---
layout: post
title:  "Perl与Ruby解析Properties文件之异同"
date:   2011-06-14 15:38:34
author: liujun.1980
categories: program
---

## Perl与Ruby解析Properties文件之异同
### by liujun.1980
### at 2011-06-14 15:38:34
### original <http://liumaodou.iteye.com/blog/1087844>

Perl解释Properties不太方便，需要自己分析
<br><pre name="code">
#!/usr/bin/perl -w

package ncs::Properties;

use vars qw(@ISA @EXPORT @EXPORT_OK);
use Exporter;
@ISA = qw(Exporter);
@EXPORT = qw(load get set merge merge_with_file);

use ncs::Common;

sub new
{
    my $this = {};
    $this-&gt;{&#39;properties_file&#39;} = &#39;ncs_default.properties&#39;;
    $this-&gt;{&#39;properties&#39;} = {};
    bless $this;
    return $this;
}

sub load
{
    my ($class, $properties_file, $keep_escape) = @_;
    if (!defined($properties_file) || $properties_file eq &quot;&quot;){ 
        $properties_file = &quot;ncs.properties&quot;;
    }
    $class-&gt;{&#39;properties_file&#39;} = $properties_file;
    my $k = $v = &quot;&quot;;
	my $is_multiline = 0;
    open(PROPFILE, &quot;&lt;$properties_file&quot;) || die(&quot;Cannot open properties file $properties_file&quot;);
    while($line = &lt;PROPFILE&gt;){
		$line = trim($line);
        #space line or commentted line will be ignored
        if($line =~ /^$/ || $line =~ /^#/){ 
            next;  
        }
		if($line =~ /\\$/){ #end with \
			$line =~ s/\\$//;
			if($is_multiline){ #not first line,not end line
				$v = $v.&#39; &#39;.$line;
			}
			else{ #first line
				($k,$v) = split(&quot;=&quot;, $line, 2);
				$is_multiline = 1;
			}
			next;
		}
		if($is_multiline){
			#print &quot;is multiline: $is_multiline\n&quot;;
			#print &quot;$line\n&quot;;
			$v = $v.&#39; &#39;.$line;
			if($line =~ /[^\\]$/){ #end line
				$class-&gt;{&#39;properties&#39;}-&gt;{trim($k)} = trim($v);
				#print &quot;$k=$v\n&quot;;
				$k = $v = &quot;&quot;;
				$is_multiline = 0;
			}
			next;
		}
        ($k,$v) = split(&quot;=&quot;, $line, 2);
        if(trim($k) eq &quot;&quot;){
            next;
        }
        #print(&quot;$k=$v&quot;);
        $class-&gt;{&#39;properties&#39;}-&gt;{trim($k)} = trim($v);
		$k = $v = &quot;&quot;;
    }
    close(PROPFILE);
	my $import = $class-&gt;get(&quot;ncs.import&quot;);
	$import = $class-&gt;unescapeProperty(&quot;ncs.import&quot;, $import) if $import;
	if(-e $import){
		print(&quot;found import properties: $import\n&quot;);
		$class-&gt;merge_with_file($import);
	}
	$class-&gt;unescapeProperties() if !$keep_escape;
    return $class-&gt;{&#39;properties&#39;};
}

sub unescapeProperty
{
    my ($class, $k, $v) = @_;
    if(!defined($v) || $v =~ /^\s*$/){ 
        return;
    }
    
    while($v =~ /^(.*)\${([^}]+)}(.*)$/){ #match ${ncs.log.dir}
        my $prefix = (defined($1) ? $1 : &quot;&quot;);
        my $matched = $2;
        my $suffix = (defined($3) ? $3 : &quot;&quot;);
        #print(&quot;matched key: $matched\n&quot;);
        if(!exists($class-&gt;{&#39;properties&#39;}-&gt;{$matched})){
			warn(&quot;[warn] matched key not exists: $matched&quot;);
			last;
		}
		#matched key exits
		#print(&quot;$matched=&quot;.$class-&gt;{&#39;properties&#39;}-&gt;{$matched}.&quot;\n&quot;);
		$class-&gt;{&#39;properties&#39;}-&gt;{$k} = $prefix.$class-&gt;{&#39;properties&#39;}-&gt;{$matched}.$suffix;
		$v = $class-&gt;{&#39;properties&#39;}-&gt;{$k};
    }
    
    while($v =~ /^(.*)(\$ENV{[^}]+})(.*)$/){ #match $ENV{HOME}
        my $prefix = (defined($1) ? $1 : &quot;&quot;);
        my $matched = $2;
        my $suffix = (defined($3) ? $3 : &quot;&quot;);
		#print(&quot;matched env: $matched\n&quot;);
        $matched = eval($matched) || &#39;&#39;;
		#print(&quot;matched env value:&quot;.$matched.&quot;\n&quot;);
        $matched = (defined($matched) ? $matched : &quot;&quot;);
        $class-&gt;{&#39;properties&#39;}{$k} = $prefix.$matched.$suffix;
        $v = $class-&gt;{&#39;properties&#39;}-&gt;{$k};
    }
    #print(&quot;$k=$class-&gt;{&#39;properties&#39;}-&gt;{$k}\n&quot;);
	return $v;
}

sub unescapeProperties
{
    my $class = shift @_;
    my $props = $class-&gt;{&#39;properties&#39;};
	#print &quot;list properties before unescape start\n&quot;;
    while( local ($k,$v) = each(%$props)){
		#print(&quot;$k=$v\n&quot;);
        $class-&gt;unescapeProperty($k, $v);
    }
	#print &quot;list properties before unescape end\n&quot;;
}

sub get
{
    my ($class,$key,$default) = @_;
    if(exists($class-&gt;{&#39;properties&#39;}-&gt;{$key})){
        return $class-&gt;{&#39;properties&#39;}-&gt;{$key};
    }
    if(defined($default)){ #not exists $props{$key}
        return $default;
    }
    return &quot;&quot;; #no default
}

sub set
{
    my ($class, $key, $value) = @_;
    $class-&gt;{&#39;properties&#39;}-&gt;{$key} = $value;
}

sub merge
{
	my ($class, $props_to_be_merged, $override) = @_;
	my $props = $class-&gt;{&#39;properties&#39;};
	while(local ($k,$v) = each(%$props_to_be_merged)){
		$k = trim($k);
		if($k eq &quot;&quot;){ next; }
        if(exists($props-&gt;{$k})){ next if(!$override); }
		$props-&gt;{$k} = trim($v);
    }
    return $props;
}

sub merge_with_file
{
	my ($class, $file_to_be_merged, $override) = @_;
	if(-e $file_to_be_merged){
		return $class-&gt;merge(ncs::Properties-&gt;new()-&gt;load($file_to_be_merged, 1), $override);
	}
	return $class-&gt;{&#39;properties&#39;};
}

</pre>
<br>Ruby也没有提供现成的库，自己写了一个，可以看出，ruby跟perl是何等的相似，尤其是正则表达式的应用这块，难怪网络上都说ruby是perl6，确实不假。
<br><pre name="code">
#!/usr/bin/ruby -w

class Properties
	def initialize
		@properties = {}
	end
	
	def load(properties_file, escape=true)
		raise &quot;#{properties_file} not exists!&quot; if not File.exists?(properties_file)
		@properties_file = properties_file
		k = v = &quot;&quot;
		is_multiline = false
		IO::foreach(@properties_file) {|line|
			line = line.strip
			#skip blank-space line or comments 
			next if line =~/^\s*$/ or line =~/^\s*#/
			#puts &quot;line: #{line}&quot;
			#end with \
			if line =~ /\\$/
				#puts &quot;line end with \\: #{line}&quot;
				line.chomp!().chop!()
				if is_multiline #not first line,not end line
					v = v + &#39; &#39; + line
				else #first line
					k,v=line.split(/=/, 2)
					is_multiline = true
				end
				next;
			end
			if is_multiline
				#puts &quot;line is one of multiline: #{is_multiline}&quot;
				v = v+&#39; &#39;+line
				if line =~ /[^\\]$/ #end line
					@properties.store(k.strip, v.strip)
					#puts &quot;#{k}=#{v}&quot;
					k = v = &quot;&quot;
					is_multiline = false
				end
				next;
			end
			k,v=line.split(/=/, 2)
			next if(k.strip.empty?)
			#puts &quot;#{k}=#{v}&quot;
			@properties.store(k.strip, v.strip)
			k = v = &quot;&quot;
		}
		self.import_files()
		self._escapeprops() if escape
		return self
	end
	
	def _escapeprops
		@properties.each do |key,val|
			self._escapeprop(key,val)
		end
	end
	def _escapeenv(env)
		env = env.delete(&#39;$&#39;).sub(&#39;{&#39;,&#39;[&quot;&#39;).sub(&#39;}&#39;,&#39;&quot;]&#39;)
		#puts &quot;env: #{matched}&quot;
		(eval(env) or &quot;&quot;)
	end
	def _escapeprop(key,val)
		return if val =~ /^\s*$/ #ignore empty value
		while val =~ /^(.*)\$\{([^}]+)\}(.*)$/ #match ${ncs.log.dir}
			prefix = $1 or &quot;&quot;; matched = $2; suffix = $3 or &quot;&quot;
			#puts &quot;matched key: #{matched}&quot;
			if not @properties.key?(matched)
				puts &quot;matched key not exists: #{matched}&quot;
				break
			end
			#matched key exits
			#puts &quot;#{matched}=&quot;+ @properties.fetch(matched)
			@properties.store(key, prefix+@properties.fetch(matched)+suffix)
			val = @properties.fetch(key)
		end
		
		while(val =~ /^(.*)(\$ENV\{[^}]+\})(.*)$/) #match $ENV{HOME}
			prefix = $1 or &quot;&quot;; matched = $2; suffix = $3 or &quot;&quot;
			matched = self._escapeenv(matched)
			#puts &quot;matched env value:#{matched}&quot;
			#puts &quot;prefix=#{prefix}, matched=#{matched}, suffix=#{suffix}&quot;
			@properties.store(key, prefix+matched+suffix)
			val = @properties.fetch(key)
		end
		#puts &quot;#{key}=#{@properties[key]}&quot;
		return val;
	end
	
	def get(key, default=&#39;&#39;)
		if(@properties.key?(key)): return @properties.fetch(key) end
		if(!default.nil?): return default end
	end
	
	def getint(key, default=0)
		self.get(key,default).to_i
	end
	def getfloat(key, default=0.0)
		self.get(key,default).to_f
	end
	
	alias getlong getint
	alias getdouble getfloat
	
	def getboolean(key)
		return true if self.getint(key, 0) &gt; 0
		return false
	end
	
	def set(key,value)
		@properties.store(key,value)
	end
	
	def size
		@properties.size
	end
	
	def merge(props_to_be_merged, override=false)
		props_to_be_merged.each do |k,v|
			next if k.strip.empty?
			next if @properties.key?(k) and !override
			@properties.store(k, v.strip)
		end
		self
	end

	def import_files(override=false)
		import = self.get(&quot;ncs.import&quot;)
		if defined?(import) and not import.empty?
			puts &quot;found import properties: #{import}&quot;
			import = self._escapeprop(&quot;ncs.import&quot;,import)
			if not import.nil? and File.exist?(import)
				return self.merge(Properties.new.load(import,false).to_hash, override)
			end
		end
		self
	end

	def to_hash
		@properties
	end
	def keys
		@properties.keys
	end
	def values
		@properties.values
	end
	def dump
		keys = @properties.keys.sort
		keys.each do |key|
			val = self.get(key)
			puts &quot;#{key}=#{val}&quot;
		end
	end
	protected :_escapeprop,:_escapeenv,:_escapeprops
end

</pre>
<br>其实还写了一版python的，也一并贴上来对比一下
<br><pre name="code">
#!/usr/bin/env python

from ncs.core.component import Component
from ncs.exceptions import *

import sys
import os
import re
import logging

class PropertiesNotExist(Exception):
	def __init__(self, properties_file):
		self.properties_file = properties_file
	
	def __str__(self):
		return repr("properties file {0} not exist!".format(self.properties_file)) 
		
class PropertiesNotLoad(Exception):
	def __init__(self, properties_file):
		self.properties_file = properties_file
	
	def __str__(self):
		return repr("properties file {0} is not loaded!".format(self.properties_file)) 
		
class Properties(Component):
    """
    This is a python portion for java Properties file
    """
    
    def __init__(self):
        Component.__init__(self, 'ncs.core.properties.Properties', ['load','get','set','search'])
        self.properties_file = ''
        self.raw_properties = {}
        self.properties = {}
        self.logger = logging.getLogger(self.__class__.__name__)
        
    def load(self, resource):
        if not os.path.exists(resource):
            self.logger.error("{0} not exists".format(resource))
            raise PropertiesNotExist(resource)
            
        if not os.path.isfile(resource):
            self.logger.error("{0} is not a file!".format(resource))
            raise PropertiesNotLoad(resource)
        
        self.properties_file = resource
        self.raw_properties = self.properties = self._parse(resource)
        #check if there is import properties or file
        import_file = self.get("ncs.import", self.get("import"))
        if import_file: 
            import_file = self._unescape_property(import_file)
            if not os.path.exists(import_file):
                self.logger.warn("import file {0} not exists".format(import_file))
                self.logger.warn("ignored import file {0}".format(import_file))
            if self.isdebugon(): print("import properties file: {0}".format(import_file)) 
            self.import_from_file(import_file)
        self._unescape_properties()
        return self.properties
    
    def _parse(self, resource):
        props = dict()
        fp = open(resource, 'r')
        (k,v) = ('','')
        is_multi_line = False
        for line in fp.readlines():
            line = line.strip()
             #space line or commentted line will be ignored
            if len(line) == 0 or line.startswith('#'):
                continue
            #end with character(\), multi-line start line
            if not is_multi_line and line.endswith('\\'):
                is_multi_line = True
                #remove the end character(\) and split with character(=)
                (k,v) = line.replace('\\','').split('=', 1)
                continue
            if is_multi_line:
                #not end with character(\), multi-line end line
                if not line.endswith('\\'):
                    v = v + ' ' + line
                    props[k.strip()] = v.strip()
                    k = v = ''
                    is_multi_line = False
                #multi-line
                else:
                    v = v + ' ' + line.replace('\\','')
                continue
            #normail line(key=value)
            (k,v) = line.replace('\\','').split('=', 1)
            if len(k.strip()) == 0:
                continue
            props[k.strip()] = v.strip()
            (k,v) = ('','')
        return props
        
    def _unescape_property(self, value):
        #white space
        if len(value.strip()) == 0: return
        
        #match ${ncs.log.dir}
        extract_vars = lambda s: [v[1] for v in re.findall(r'^(.*)\${([^}]+)}(.*)$', s)]
        while extract_vars(value):
            for var in extract_vars(value):
                if self.isdebugon(): print("matched key: {0}".format(var))
                if not self.properties.has_key(var):
                    print("[warn] matched key not exists: {0}".format(var))
                    break
                #matched key exits
                val = self.properties.get(var)
                if self.isdebugon(): print("{0} = {1}".format(var, val))
                value = value.replace('${'+var+'}', val)
            
            
        #match $ENV{HOME}
        extract_envs = lambda s: [e[1] for e in re.findall(r'^(.*)\$ENV{([^}]+)}(.*)$', s)]
        while extract_envs(value):
            for env in extract_envs(value):
                if self.isdebugon(): print("matched environ: {0} = {1}".format(env, os.getenv(env,'')))
                value = value.replace('$ENV{'+env+'}', os.getenv(env,''))
            
        return value
        
    def _unescape_properties(self):
        for (key,value) in self.properties.items():
            self.properties[key] = self._unescape_property(value)
        
    def get(self, key, default=None):
        return self.properties.get(key, default)
    def getint(self, key, default=0):
        int(self.get(key,default))
    def getlong(self, key, default=0):
        long(self.get(key,default))
    def getfloat(self, key, default=0.0):
        float(self.get(key,default))
    def getboolean(self, key, default=False):
        value = self.get(key)
        if value: 
            return True
        return default or False
    def getrange(self, key):
        result = []
        value = self.get(key)
        if value:
            for val in value.split(','):
                #val as range
                if '-' in val:
                    (start,end) = val.split('-', 1)
                    result.extend(range(int(start),int(end)+1))
                else:
                    result.append(int(val))
        return result

    def set(self, key, value=None):
        self.properties[key] = value

    def search(self, keywords):
        props = {}
        for (key,value) in self.properties.items():
            if keywords in key:
                props[key] = value
        return props
	
    def size(self):
        return len(self.properties.items())
        
    def dump(self):
        for key in sorted(self.properties.keys()): print("{0} = {1}".format(key,self.properties.get(key)))
            
    def import_properties(self, properties, override=False):
        for (key,value) in properties.items():
            if self.properties.has_key(key) and not override: continue 
            self.properties[key] = value
            
    def import_from_file(self, resource, override=False):
        properties = self._parse(resource)
        self.import_properties(properties, override)
	
</pre>
          
          <br><br>
          <span style="color:red">
            <a href="http://liumaodou.iteye.com/blog/1087844#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>