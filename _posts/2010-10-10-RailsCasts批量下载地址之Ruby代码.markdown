---
layout: post
title:  "RailsCasts批量下载地址之Ruby代码"
date:   2010-10-10 14:19:00
author: 王德水
categories: program
---

## RailsCasts批量下载地址之Ruby代码
### by 王德水
### at 2010-10-10 14:19:00
### original <http://www.cnblogs.com/cnblogsfans/archive/2010/10/10/1847174.html>

<p><a href="http://www.cnblogs.com/cnblogsfans/"><img src="http://pic.cnblogs.com/face/u26781.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/cnblogsfans/">王德水</a> 发表于 2010-10-10 14:19 <a href="http://www.cnblogs.com/cnblogsfans/archive/2010/10/10/1847174.html">原文链接</a> 阅读: 457 评论: 3</p><p>千呼万唤的Rails3出来了，也该开始学学了，从网上发现一个好的教程<a href="http://railscasts.com/episodes">http://railscasts.com/episodes</a>，能够下载</p>  <p><a href="http://images.cnblogs.com/cnblogs_com/cnblogsfans/WindowsLiveWriter/RailsCastsRuby_C8B0/image_2.png"><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="image" border="0" alt="image" src="http://images.cnblogs.com/cnblogs_com/cnblogsfans/WindowsLiveWriter/RailsCastsRuby_C8B0/image_thumb.png" width="556" height="275"></a> </p>  <p>但遗憾的是每页只显示10个而且无法批量下载，如是发现右边栏有All Episodes链接。</p>  <p><a href="http://images.cnblogs.com/cnblogs_com/cnblogsfans/WindowsLiveWriter/RailsCastsRuby_C8B0/image_4.png"><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="image" border="0" alt="image" src="http://images.cnblogs.com/cnblogs_com/cnblogsfans/WindowsLiveWriter/RailsCastsRuby_C8B0/image_thumb_1.png" width="356" height="355"></a> </p>  <p>但是这个没有下载地址，只能一个个点进去才能看见下载地址。仔细对比这两个地址</p>  <p><a title="http://railscasts.com/episodes/234-simple-form" href="http://railscasts.com/episodes/234-simple-form">http://railscasts.com/episodes/234-simple-form</a></p>  <p><a title="http://media.railscasts.com/videos/234_simple_form.mov" href="http://media.railscasts.com/videos/234_simple_form.mov">http://media.railscasts.com/videos/234_simple_form.mov</a></p>  <p>发现他们之间有一定的对应关系，狂喜，于是有了如下代码</p>  <pre>require &#39;open-uri&#39;
open(&#39;http://railscasts.com/episodes/archive&#39;) do |f|
  s=&quot;&quot;
  f.each do |line|
    s&lt;&lt;line
  end

  allUrls=File.new(File.join(&quot;C:&quot;, &quot;RailCastsVideoURLs.txt&quot;), &quot;w+&quot;)
  m=/href=&quot;(\/episodes\/.+)&quot;/
  urls= s.scan(m)
  urls.each { |x|
    begin
     allUrls.puts x[0].gsub(/\/episodes\//, &quot;http://media.railscasts.com/videos/&quot;).gsub(/-/,&quot;_&quot;).to_s+&quot;.mov&quot;
    end
  }
  allUrls.close
end</pre>

<p> </p>

<p>运行以上代码，可以得到要下载的所有地址，当然前提是你要安装ruby, 如果你没装，那我附上所有的地址吧！找到地址后我相信大家知道怎么下载了，你可以写段Ruby脚本下载，当然我更喜欢用迅雷呀。</p>

<p><a href="http://media.railscasts.com/videos/234_simple_form.mov">http://media.railscasts.com/videos/234_simple_form.mov</a> 

  <br><a href="http://media.railscasts.com/videos/233_engage_with_devise.mov">http://media.railscasts.com/videos/233_engage_with_devise.mov</a> 

  <br><a href="http://media.railscasts.com/videos/232_routing_walkthrough_part_2.mov">http://media.railscasts.com/videos/232_routing_walkthrough_part_2.mov</a> 

  <br><a href="http://media.railscasts.com/videos/231_routing_walkthrough.mov">http://media.railscasts.com/videos/231_routing_walkthrough.mov</a> 

  <br><a href="http://media.railscasts.com/videos/230_inherited_resources.mov">http://media.railscasts.com/videos/230_inherited_resources.mov</a> 

  <br><a href="http://media.railscasts.com/videos/229_polling_for_changes.mov">http://media.railscasts.com/videos/229_polling_for_changes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/228_sortable_table_columns.mov">http://media.railscasts.com/videos/228_sortable_table_columns.mov</a> 

  <br><a href="http://media.railscasts.com/videos/227_upgrading_to_rails_3_part_3.mov">http://media.railscasts.com/videos/227_upgrading_to_rails_3_part_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/226_upgrading_to_rails_3_part_2.mov">http://media.railscasts.com/videos/226_upgrading_to_rails_3_part_2.mov</a> 

  <br><a href="http://media.railscasts.com/videos/225_upgrading_to_rails_3_part_1.mov">http://media.railscasts.com/videos/225_upgrading_to_rails_3_part_1.mov</a> 

  <br><a href="http://media.railscasts.com/videos/224_controllers_in_rails_3.mov">http://media.railscasts.com/videos/224_controllers_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/223_charts.mov">http://media.railscasts.com/videos/223_charts.mov</a> 

  <br><a href="http://media.railscasts.com/videos/222_rack_in_rails_3.mov">http://media.railscasts.com/videos/222_rack_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/221_subdomains_in_rails_3.mov">http://media.railscasts.com/videos/221_subdomains_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/220_pdfkit.mov">http://media.railscasts.com/videos/220_pdfkit.mov</a> 

  <br><a href="http://media.railscasts.com/videos/219_active_model.mov">http://media.railscasts.com/videos/219_active_model.mov</a> 

  <br><a href="http://media.railscasts.com/videos/218_making_generators_in_rails_3.mov">http://media.railscasts.com/videos/218_making_generators_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/217_multistep_forms.mov">http://media.railscasts.com/videos/217_multistep_forms.mov</a> 

  <br><a href="http://media.railscasts.com/videos/216_generators_in_rails_3.mov">http://media.railscasts.com/videos/216_generators_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/215_advanced_queries_in_rails_3.mov">http://media.railscasts.com/videos/215_advanced_queries_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/214_a_b_testing_with_a_bingo.mov">http://media.railscasts.com/videos/214_a_b_testing_with_a_bingo.mov</a> 

  <br><a href="http://media.railscasts.com/videos/213_calendars.mov">http://media.railscasts.com/videos/213_calendars.mov</a> 

  <br><a href="http://media.railscasts.com/videos/212_refactoring_dynamic_delegator.mov">http://media.railscasts.com/videos/212_refactoring_dynamic_delegator.mov</a> 

  <br><a href="http://media.railscasts.com/videos/211_validations_in_rails_3.mov">http://media.railscasts.com/videos/211_validations_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/210_customizing_devise.mov">http://media.railscasts.com/videos/210_customizing_devise.mov</a> 

  <br><a href="http://media.railscasts.com/videos/209_introducing_devise.mov">http://media.railscasts.com/videos/209_introducing_devise.mov</a> 

  <br><a href="http://media.railscasts.com/videos/208_erb_blocks_in_rails_3.mov">http://media.railscasts.com/videos/208_erb_blocks_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/207_syntax_highlighting.mov">http://media.railscasts.com/videos/207_syntax_highlighting.mov</a> 

  <br><a href="http://media.railscasts.com/videos/206_action_mailer_in_rails_3.mov">http://media.railscasts.com/videos/206_action_mailer_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/205_unobtrusive_javascript.mov">http://media.railscasts.com/videos/205_unobtrusive_javascript.mov</a> 

  <br><a href="http://media.railscasts.com/videos/204_xss_protection_in_rails_3.mov">http://media.railscasts.com/videos/204_xss_protection_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/203_routing_in_rails_3.mov">http://media.railscasts.com/videos/203_routing_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/202_active_record_queries_in_rails_3.mov">http://media.railscasts.com/videos/202_active_record_queries_in_rails_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/201_bundler.mov">http://media.railscasts.com/videos/201_bundler.mov</a> 

  <br><a href="http://media.railscasts.com/videos/200_rails_3_beta_and_rvm.mov">http://media.railscasts.com/videos/200_rails_3_beta_and_rvm.mov</a> 

  <br><a href="http://media.railscasts.com/videos/199_mobile_devices.mov">http://media.railscasts.com/videos/199_mobile_devices.mov</a> 

  <br><a href="http://media.railscasts.com/videos/198_edit_multiple_individually.mov">http://media.railscasts.com/videos/198_edit_multiple_individually.mov</a> 

  <br><a href="http://media.railscasts.com/videos/197_nested_model_form_part_2.mov">http://media.railscasts.com/videos/197_nested_model_form_part_2.mov</a> 

  <br><a href="http://media.railscasts.com/videos/196_nested_model_form_part_1.mov">http://media.railscasts.com/videos/196_nested_model_form_part_1.mov</a> 

  <br><a href="http://media.railscasts.com/videos/195_my_favorite_web_apps_in_2009.mov">http://media.railscasts.com/videos/195_my_favorite_web_apps_in_2009.mov</a> 

  <br><a href="http://media.railscasts.com/videos/194_mongodb_and_mongomapper.mov">http://media.railscasts.com/videos/194_mongodb_and_mongomapper.mov</a> 

  <br><a href="http://media.railscasts.com/videos/193_tableless_model.mov">http://media.railscasts.com/videos/193_tableless_model.mov</a> 

  <br><a href="http://media.railscasts.com/videos/192_authorization_with_cancan.mov">http://media.railscasts.com/videos/192_authorization_with_cancan.mov</a> 

  <br><a href="http://media.railscasts.com/videos/191_mechanize.mov">http://media.railscasts.com/videos/191_mechanize.mov</a> 

  <br><a href="http://media.railscasts.com/videos/190_screen_scraping_with_nokogiri.mov">http://media.railscasts.com/videos/190_screen_scraping_with_nokogiri.mov</a> 

  <br><a href="http://media.railscasts.com/videos/189_embedded_association.mov">http://media.railscasts.com/videos/189_embedded_association.mov</a> 

  <br><a href="http://media.railscasts.com/videos/188_declarative_authorization.mov">http://media.railscasts.com/videos/188_declarative_authorization.mov</a> 

  <br><a href="http://media.railscasts.com/videos/187_testing_exceptions.mov">http://media.railscasts.com/videos/187_testing_exceptions.mov</a> 

  <br><a href="http://media.railscasts.com/videos/186_pickle_with_cucumber.mov">http://media.railscasts.com/videos/186_pickle_with_cucumber.mov</a> 

  <br><a href="http://media.railscasts.com/videos/185_formtastic_part_2.mov">http://media.railscasts.com/videos/185_formtastic_part_2.mov</a> 

  <br><a href="http://media.railscasts.com/videos/184_formtastic_part_1.mov">http://media.railscasts.com/videos/184_formtastic_part_1.mov</a> 

  <br><a href="http://media.railscasts.com/videos/183_gemcutter_jeweler.mov">http://media.railscasts.com/videos/183_gemcutter_jeweler.mov</a> 

  <br><a href="http://media.railscasts.com/videos/182_cropping_images.mov">http://media.railscasts.com/videos/182_cropping_images.mov</a> 

  <br><a href="http://media.railscasts.com/videos/181_include_vs_joins.mov">http://media.railscasts.com/videos/181_include_vs_joins.mov</a> 

  <br><a href="http://media.railscasts.com/videos/180_finding_unused_css.mov">http://media.railscasts.com/videos/180_finding_unused_css.mov</a> 

  <br><a href="http://media.railscasts.com/videos/179_seed_data.mov">http://media.railscasts.com/videos/179_seed_data.mov</a> 

  <br><a href="http://media.railscasts.com/videos/178_seven_security_tips.mov">http://media.railscasts.com/videos/178_seven_security_tips.mov</a> 

  <br><a href="http://media.railscasts.com/videos/177_model_versioning.mov">http://media.railscasts.com/videos/177_model_versioning.mov</a> 

  <br><a href="http://media.railscasts.com/videos/176_searchlogic.mov">http://media.railscasts.com/videos/176_searchlogic.mov</a> 

  <br><a href="http://media.railscasts.com/videos/175_ajax_history_and_bookmarks.mov">http://media.railscasts.com/videos/175_ajax_history_and_bookmarks.mov</a> 

  <br><a href="http://media.railscasts.com/videos/174_pagination_with_ajax.mov">http://media.railscasts.com/videos/174_pagination_with_ajax.mov</a> 

  <br><a href="http://media.railscasts.com/videos/173_screen_scraping_with_scrapi.mov">http://media.railscasts.com/videos/173_screen_scraping_with_scrapi.mov</a> 

  <br><a href="http://media.railscasts.com/videos/172_touch_and_cache.mov">http://media.railscasts.com/videos/172_touch_and_cache.mov</a> 

  <br><a href="http://media.railscasts.com/videos/171_delayed_job.mov">http://media.railscasts.com/videos/171_delayed_job.mov</a> 

  <br><a href="http://media.railscasts.com/videos/170_openid_with_authlogic.mov">http://media.railscasts.com/videos/170_openid_with_authlogic.mov</a> 

  <br><a href="http://media.railscasts.com/videos/169_dynamic_page_caching.mov">http://media.railscasts.com/videos/169_dynamic_page_caching.mov</a> 

  <br><a href="http://media.railscasts.com/videos/168_feed_parsing.mov">http://media.railscasts.com/videos/168_feed_parsing.mov</a> 

  <br><a href="http://media.railscasts.com/videos/167_more_on_virtual_attributes.mov">http://media.railscasts.com/videos/167_more_on_virtual_attributes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/166_metric_fu.mov">http://media.railscasts.com/videos/166_metric_fu.mov</a> 

  <br><a href="http://media.railscasts.com/videos/165_edit_multiple.mov">http://media.railscasts.com/videos/165_edit_multiple.mov</a> 

  <br><a href="http://media.railscasts.com/videos/164_cron_in_ruby.mov">http://media.railscasts.com/videos/164_cron_in_ruby.mov</a> 

  <br><a href="http://media.railscasts.com/videos/163_self_referential_association.mov">http://media.railscasts.com/videos/163_self_referential_association.mov</a> 

  <br><a href="http://media.railscasts.com/videos/162_tree_based_navigation.mov">http://media.railscasts.com/videos/162_tree_based_navigation.mov</a> 

  <br><a href="http://media.railscasts.com/videos/161_three_profiling_tools.mov">http://media.railscasts.com/videos/161_three_profiling_tools.mov</a> 

  <br><a href="http://media.railscasts.com/videos/160_authlogic.mov">http://media.railscasts.com/videos/160_authlogic.mov</a> 

  <br><a href="http://media.railscasts.com/videos/159_more_on_cucumber.mov">http://media.railscasts.com/videos/159_more_on_cucumber.mov</a> 

  <br><a href="http://media.railscasts.com/videos/158_factories_not_fixtures.mov">http://media.railscasts.com/videos/158_factories_not_fixtures.mov</a> 

  <br><a href="http://media.railscasts.com/videos/157_rspec_matchers_macros.mov">http://media.railscasts.com/videos/157_rspec_matchers_macros.mov</a> 

  <br><a href="http://media.railscasts.com/videos/156_webrat.mov">http://media.railscasts.com/videos/156_webrat.mov</a> 

  <br><a href="http://media.railscasts.com/videos/155_beginning_with_cucumber.mov">http://media.railscasts.com/videos/155_beginning_with_cucumber.mov</a> 

  <br><a href="http://media.railscasts.com/videos/154_polymorphic_association.mov">http://media.railscasts.com/videos/154_polymorphic_association.mov</a> 

  <br><a href="http://media.railscasts.com/videos/153_pdfs_with_prawn.mov">http://media.railscasts.com/videos/153_pdfs_with_prawn.mov</a> 

  <br><a href="http://media.railscasts.com/videos/152_rails_2_3_extras.mov">http://media.railscasts.com/videos/152_rails_2_3_extras.mov</a> 

  <br><a href="http://media.railscasts.com/videos/151_rack_middleware.mov">http://media.railscasts.com/videos/151_rack_middleware.mov</a> 

  <br><a href="http://media.railscasts.com/videos/150_rails_metal.mov">http://media.railscasts.com/videos/150_rails_metal.mov</a> 

  <br><a href="http://media.railscasts.com/videos/149_rails_engines.mov">http://media.railscasts.com/videos/149_rails_engines.mov</a> 

  <br><a href="http://media.railscasts.com/videos/148_app_templates_in_rails_2_3.mov">http://media.railscasts.com/videos/148_app_templates_in_rails_2_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/147_sortable_lists.mov">http://media.railscasts.com/videos/147_sortable_lists.mov</a> 

  <br><a href="http://media.railscasts.com/videos/146_paypal_express_checkout.mov">http://media.railscasts.com/videos/146_paypal_express_checkout.mov</a> 

  <br><a href="http://media.railscasts.com/videos/145_integrating_active_merchant.mov">http://media.railscasts.com/videos/145_integrating_active_merchant.mov</a> 

  <br><a href="http://media.railscasts.com/videos/144_active_merchant_basics.mov">http://media.railscasts.com/videos/144_active_merchant_basics.mov</a> 

  <br><a href="http://media.railscasts.com/videos/143_paypal_security.mov">http://media.railscasts.com/videos/143_paypal_security.mov</a> 

  <br><a href="http://media.railscasts.com/videos/142_paypal_notifications.mov">http://media.railscasts.com/videos/142_paypal_notifications.mov</a> 

  <br><a href="http://media.railscasts.com/videos/141_paypal_basics.mov">http://media.railscasts.com/videos/141_paypal_basics.mov</a> 

  <br><a href="http://media.railscasts.com/videos/140_rails_2_2_extras.mov">http://media.railscasts.com/videos/140_rails_2_2_extras.mov</a> 

  <br><a href="http://media.railscasts.com/videos/139_nested_resources.mov">http://media.railscasts.com/videos/139_nested_resources.mov</a> 

  <br><a href="http://media.railscasts.com/videos/138_i18n.mov">http://media.railscasts.com/videos/138_i18n.mov</a> 

  <br><a href="http://media.railscasts.com/videos/137_memoization.mov">http://media.railscasts.com/videos/137_memoization.mov</a> 

  <br><a href="http://media.railscasts.com/videos/136_jquery.mov">http://media.railscasts.com/videos/136_jquery.mov</a> 

  <br><a href="http://media.railscasts.com/videos/135_making_a_gem.mov">http://media.railscasts.com/videos/135_making_a_gem.mov</a> 

  <br><a href="http://media.railscasts.com/videos/134_paperclip.mov">http://media.railscasts.com/videos/134_paperclip.mov</a> 

  <br><a href="http://media.railscasts.com/videos/133_capistrano_tasks.mov">http://media.railscasts.com/videos/133_capistrano_tasks.mov</a> 

  <br><a href="http://media.railscasts.com/videos/132_helpers_outside_views.mov">http://media.railscasts.com/videos/132_helpers_outside_views.mov</a> 

  <br><a href="http://media.railscasts.com/videos/131_going_back.mov">http://media.railscasts.com/videos/131_going_back.mov</a> 

  <br><a href="http://media.railscasts.com/videos/130_monitoring_with_god.mov">http://media.railscasts.com/videos/130_monitoring_with_god.mov</a> 

  <br><a href="http://media.railscasts.com/videos/129_custom_daemon.mov">http://media.railscasts.com/videos/129_custom_daemon.mov</a> 

  <br><a href="http://media.railscasts.com/videos/128_starling_and_workling.mov">http://media.railscasts.com/videos/128_starling_and_workling.mov</a> 

  <br><a href="http://media.railscasts.com/videos/127_rake_in_background.mov">http://media.railscasts.com/videos/127_rake_in_background.mov</a> 

  <br><a href="http://media.railscasts.com/videos/126_populating_a_database.mov">http://media.railscasts.com/videos/126_populating_a_database.mov</a> 

  <br><a href="http://media.railscasts.com/videos/125_dynamic_layouts.mov">http://media.railscasts.com/videos/125_dynamic_layouts.mov</a> 

  <br><a href="http://media.railscasts.com/videos/124_beta_invitations.mov">http://media.railscasts.com/videos/124_beta_invitations.mov</a> 

  <br><a href="http://media.railscasts.com/videos/123_subdomains.mov">http://media.railscasts.com/videos/123_subdomains.mov</a> 

  <br><a href="http://media.railscasts.com/videos/122_passenger_in_development.mov">http://media.railscasts.com/videos/122_passenger_in_development.mov</a> 

  <br><a href="http://media.railscasts.com/videos/121_non_active_record_model.mov">http://media.railscasts.com/videos/121_non_active_record_model.mov</a> 

  <br><a href="http://media.railscasts.com/videos/120_thinking_sphinx.mov">http://media.railscasts.com/videos/120_thinking_sphinx.mov</a> 

  <br><a href="http://media.railscasts.com/videos/119_session_based_model.mov">http://media.railscasts.com/videos/119_session_based_model.mov</a> 

  <br><a href="http://media.railscasts.com/videos/118_liquid.mov">http://media.railscasts.com/videos/118_liquid.mov</a> 

  <br><a href="http://media.railscasts.com/videos/117_semi_static_pages.mov">http://media.railscasts.com/videos/117_semi_static_pages.mov</a> 

  <br><a href="http://media.railscasts.com/videos/116_selenium.mov">http://media.railscasts.com/videos/116_selenium.mov</a> 

  <br><a href="http://media.railscasts.com/videos/115_caching_in_rails_2_1.mov">http://media.railscasts.com/videos/115_caching_in_rails_2_1.mov</a> 

  <br><a href="http://media.railscasts.com/videos/114_endless_page.mov">http://media.railscasts.com/videos/114_endless_page.mov</a> 

  <br><a href="http://media.railscasts.com/videos/113_contributing_to_rails_with_git.mov">http://media.railscasts.com/videos/113_contributing_to_rails_with_git.mov</a> 

  <br><a href="http://media.railscasts.com/videos/112_anonymous_scopes.mov">http://media.railscasts.com/videos/112_anonymous_scopes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/111_advanced_search_form.mov">http://media.railscasts.com/videos/111_advanced_search_form.mov</a> 

  <br><a href="http://media.railscasts.com/videos/110_gem_dependencies.mov">http://media.railscasts.com/videos/110_gem_dependencies.mov</a> 

  <br><a href="http://media.railscasts.com/videos/109_tracking_attribute_changes.mov">http://media.railscasts.com/videos/109_tracking_attribute_changes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/108_named_scope.mov">http://media.railscasts.com/videos/108_named_scope.mov</a> 

  <br><a href="http://media.railscasts.com/videos/107_migrations_in_rails_2_1.mov">http://media.railscasts.com/videos/107_migrations_in_rails_2_1.mov</a> 

  <br><a href="http://media.railscasts.com/videos/106_time_zones_in_rails_2_1.mov">http://media.railscasts.com/videos/106_time_zones_in_rails_2_1.mov</a> 

  <br><a href="http://media.railscasts.com/videos/105_gitting_rails_2_1_rc1.mov">http://media.railscasts.com/videos/105_gitting_rails_2_1_rc1.mov</a> 

  <br><a href="http://media.railscasts.com/videos/104_exception_notifications.mov">http://media.railscasts.com/videos/104_exception_notifications.mov</a> 

  <br><a href="http://media.railscasts.com/videos/103_site_wide_announcements.mov">http://media.railscasts.com/videos/103_site_wide_announcements.mov</a> 

  <br><a href="http://media.railscasts.com/videos/102_auto_complete_association.mov">http://media.railscasts.com/videos/102_auto_complete_association.mov</a> 

  <br><a href="http://media.railscasts.com/videos/101_refactoring_out_helper_object.mov">http://media.railscasts.com/videos/101_refactoring_out_helper_object.mov</a> 

  <br><a href="http://media.railscasts.com/videos/100_5_view_tips.mov">http://media.railscasts.com/videos/100_5_view_tips.mov</a> 

  <br><a href="http://media.railscasts.com/videos/99_complex_partials.mov">http://media.railscasts.com/videos/99_complex_partials.mov</a> 

  <br><a href="http://media.railscasts.com/videos/98_request_profiling.mov">http://media.railscasts.com/videos/98_request_profiling.mov</a> 

  <br><a href="http://media.railscasts.com/videos/97_analyzing_the_production_log.mov">http://media.railscasts.com/videos/97_analyzing_the_production_log.mov</a> 

  <br><a href="http://media.railscasts.com/videos/96_git_on_rails.mov">http://media.railscasts.com/videos/96_git_on_rails.mov</a> 

  <br><a href="http://media.railscasts.com/videos/95_more_on_activeresource.mov">http://media.railscasts.com/videos/95_more_on_activeresource.mov</a> 

  <br><a href="http://media.railscasts.com/videos/94_activeresource_basics.mov">http://media.railscasts.com/videos/94_activeresource_basics.mov</a> 

  <br><a href="http://media.railscasts.com/videos/93_action_caching.mov">http://media.railscasts.com/videos/93_action_caching.mov</a> 

  <br><a href="http://media.railscasts.com/videos/92_make_resourceful.mov">http://media.railscasts.com/videos/92_make_resourceful.mov</a> 

  <br><a href="http://media.railscasts.com/videos/91_refactoring_long_methods.mov">http://media.railscasts.com/videos/91_refactoring_long_methods.mov</a> 

  <br><a href="http://media.railscasts.com/videos/90_fragment_caching.mov">http://media.railscasts.com/videos/90_fragment_caching.mov</a> 

  <br><a href="http://media.railscasts.com/videos/89_page_caching.mov">http://media.railscasts.com/videos/89_page_caching.mov</a> 

  <br><a href="http://media.railscasts.com/videos/88_dynamic_select_menus.mov">http://media.railscasts.com/videos/88_dynamic_select_menus.mov</a> 

  <br><a href="http://media.railscasts.com/videos/87_generating_rss_feeds.mov">http://media.railscasts.com/videos/87_generating_rss_feeds.mov</a> 

  <br><a href="http://media.railscasts.com/videos/86_logging_variables.mov">http://media.railscasts.com/videos/86_logging_variables.mov</a> 

  <br><a href="http://media.railscasts.com/videos/85_yaml_configuration_file.mov">http://media.railscasts.com/videos/85_yaml_configuration_file.mov</a> 

  <br><a href="http://media.railscasts.com/videos/84_cookie_based_session_store.mov">http://media.railscasts.com/videos/84_cookie_based_session_store.mov</a> 

  <br><a href="http://media.railscasts.com/videos/83_migrations_in_rails_2_0.mov">http://media.railscasts.com/videos/83_migrations_in_rails_2_0.mov</a> 

  <br><a href="http://media.railscasts.com/videos/82_http_basic_authentication.mov">http://media.railscasts.com/videos/82_http_basic_authentication.mov</a> 

  <br><a href="http://media.railscasts.com/videos/81_fixtures_in_rails_2_0.mov">http://media.railscasts.com/videos/81_fixtures_in_rails_2_0.mov</a> 

  <br><a href="http://media.railscasts.com/videos/80_simplify_views_with_rails_2_0.mov">http://media.railscasts.com/videos/80_simplify_views_with_rails_2_0.mov</a> 

  <br><a href="http://media.railscasts.com/videos/79_generate_named_routes.mov">http://media.railscasts.com/videos/79_generate_named_routes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/78_generating_pdf_documents.mov">http://media.railscasts.com/videos/78_generating_pdf_documents.mov</a> 

  <br><a href="http://media.railscasts.com/videos/77_destroy_without_javascript.mov">http://media.railscasts.com/videos/77_destroy_without_javascript.mov</a> 

  <br><a href="http://media.railscasts.com/videos/76_scope_out.mov">http://media.railscasts.com/videos/76_scope_out.mov</a> 

  <br><a href="http://media.railscasts.com/videos/75_complex_forms_part_3.mov">http://media.railscasts.com/videos/75_complex_forms_part_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/74_complex_forms_part_2.mov">http://media.railscasts.com/videos/74_complex_forms_part_2.mov</a> 

  <br><a href="http://media.railscasts.com/videos/73_complex_forms_part_1.mov">http://media.railscasts.com/videos/73_complex_forms_part_1.mov</a> 

  <br><a href="http://media.railscasts.com/videos/72_adding_an_environment.mov">http://media.railscasts.com/videos/72_adding_an_environment.mov</a> 

  <br><a href="http://media.railscasts.com/videos/71_testing_controllers_with_rspec.mov">http://media.railscasts.com/videos/71_testing_controllers_with_rspec.mov</a> 

  <br><a href="http://media.railscasts.com/videos/70_custom_routes.mov">http://media.railscasts.com/videos/70_custom_routes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/69_markaby_in_helper.mov">http://media.railscasts.com/videos/69_markaby_in_helper.mov</a> 

  <br><a href="http://media.railscasts.com/videos/68_openid_authentication.mov">http://media.railscasts.com/videos/68_openid_authentication.mov</a> 

  <br><a href="http://media.railscasts.com/videos/67_restful_authentication.mov">http://media.railscasts.com/videos/67_restful_authentication.mov</a> 

  <br><a href="http://media.railscasts.com/videos/66_custom_rake_tasks.mov">http://media.railscasts.com/videos/66_custom_rake_tasks.mov</a> 

  <br><a href="http://media.railscasts.com/videos/65_stopping_spam_with_akismet.mov">http://media.railscasts.com/videos/65_stopping_spam_with_akismet.mov</a> 

  <br><a href="http://media.railscasts.com/videos/64_custom_helper_modules.mov">http://media.railscasts.com/videos/64_custom_helper_modules.mov</a> 

  <br><a href="http://media.railscasts.com/videos/63_model_name_in_url.mov">http://media.railscasts.com/videos/63_model_name_in_url.mov</a> 

  <br><a href="http://media.railscasts.com/videos/62_hacking_activerecord.mov">http://media.railscasts.com/videos/62_hacking_activerecord.mov</a> 

  <br><a href="http://media.railscasts.com/videos/61_sending_email.mov">http://media.railscasts.com/videos/61_sending_email.mov</a> 

  <br><a href="http://media.railscasts.com/videos/60_testing_without_fixtures.mov">http://media.railscasts.com/videos/60_testing_without_fixtures.mov</a> 

  <br><a href="http://media.railscasts.com/videos/59_optimistic_locking.mov">http://media.railscasts.com/videos/59_optimistic_locking.mov</a> 

  <br><a href="http://media.railscasts.com/videos/58_how_to_make_a_generator.mov">http://media.railscasts.com/videos/58_how_to_make_a_generator.mov</a> 

  <br><a href="http://media.railscasts.com/videos/57_create_model_through_text_field.mov">http://media.railscasts.com/videos/57_create_model_through_text_field.mov</a> 

  <br><a href="http://media.railscasts.com/videos/56_the_logger.mov">http://media.railscasts.com/videos/56_the_logger.mov</a> 

  <br><a href="http://media.railscasts.com/videos/55_cleaning_up_the_view.mov">http://media.railscasts.com/videos/55_cleaning_up_the_view.mov</a> 

  <br><a href="http://media.railscasts.com/videos/54_debugging_with_ruby_debug.mov">http://media.railscasts.com/videos/54_debugging_with_ruby_debug.mov</a> 

  <br><a href="http://media.railscasts.com/videos/53_handling_exceptions.mov">http://media.railscasts.com/videos/53_handling_exceptions.mov</a> 

  <br><a href="http://media.railscasts.com/videos/52_update_through_checkboxes.mov">http://media.railscasts.com/videos/52_update_through_checkboxes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/51_will_paginate.mov">http://media.railscasts.com/videos/51_will_paginate.mov</a> 

  <br><a href="http://media.railscasts.com/videos/50_contributing_to_rails.mov">http://media.railscasts.com/videos/50_contributing_to_rails.mov</a> 

  <br><a href="http://media.railscasts.com/videos/49_reading_the_api.mov">http://media.railscasts.com/videos/49_reading_the_api.mov</a> 

  <br><a href="http://media.railscasts.com/videos/48_console_tricks.mov">http://media.railscasts.com/videos/48_console_tricks.mov</a> 

  <br><a href="http://media.railscasts.com/videos/47_two_many_to_many.mov">http://media.railscasts.com/videos/47_two_many_to_many.mov</a> 

  <br><a href="http://media.railscasts.com/videos/46_catch_all_route.mov">http://media.railscasts.com/videos/46_catch_all_route.mov</a> 

  <br><a href="http://media.railscasts.com/videos/45_rjs_tips.mov">http://media.railscasts.com/videos/45_rjs_tips.mov</a> 

  <br><a href="http://media.railscasts.com/videos/44_debugging_rjs.mov">http://media.railscasts.com/videos/44_debugging_rjs.mov</a> 

  <br><a href="http://media.railscasts.com/videos/43_ajax_with_rjs.mov">http://media.railscasts.com/videos/43_ajax_with_rjs.mov</a> 

  <br><a href="http://media.railscasts.com/videos/42_with_options.mov">http://media.railscasts.com/videos/42_with_options.mov</a> 

  <br><a href="http://media.railscasts.com/videos/41_conditional_validations.mov">http://media.railscasts.com/videos/41_conditional_validations.mov</a> 

  <br><a href="http://media.railscasts.com/videos/40_blocks_in_view.mov">http://media.railscasts.com/videos/40_blocks_in_view.mov</a> 

  <br><a href="http://media.railscasts.com/videos/39_customize_field_error.mov">http://media.railscasts.com/videos/39_customize_field_error.mov</a> 

  <br><a href="http://media.railscasts.com/videos/38_multibutton_form.mov">http://media.railscasts.com/videos/38_multibutton_form.mov</a> 

  <br><a href="http://media.railscasts.com/videos/37_simple_search_form.mov">http://media.railscasts.com/videos/37_simple_search_form.mov</a> 

  <br><a href="http://media.railscasts.com/videos/36_subversion_on_rails.mov">http://media.railscasts.com/videos/36_subversion_on_rails.mov</a> 

  <br><a href="http://media.railscasts.com/videos/35_custom_rest_actions.mov">http://media.railscasts.com/videos/35_custom_rest_actions.mov</a> 

  <br><a href="http://media.railscasts.com/videos/34_named_routes.mov">http://media.railscasts.com/videos/34_named_routes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/33_making_a_plugin.mov">http://media.railscasts.com/videos/33_making_a_plugin.mov</a> 

  <br><a href="http://media.railscasts.com/videos/32_time_in_text_field.mov">http://media.railscasts.com/videos/32_time_in_text_field.mov</a> 

  <br><a href="http://media.railscasts.com/videos/31_formatting_time.mov">http://media.railscasts.com/videos/31_formatting_time.mov</a> 

  <br><a href="http://media.railscasts.com/videos/30_pretty_page_title.mov">http://media.railscasts.com/videos/30_pretty_page_title.mov</a> 

  <br><a href="http://media.railscasts.com/videos/29_group_by_month.mov">http://media.railscasts.com/videos/29_group_by_month.mov</a> 

  <br><a href="http://media.railscasts.com/videos/28_in_groups_of.mov">http://media.railscasts.com/videos/28_in_groups_of.mov</a> 

  <br><a href="http://media.railscasts.com/videos/27_cross_site_scripting.mov">http://media.railscasts.com/videos/27_cross_site_scripting.mov</a> 

  <br><a href="http://media.railscasts.com/videos/26_hackers_love_mass_assignment.mov">http://media.railscasts.com/videos/26_hackers_love_mass_assignment.mov</a> 

  <br><a href="http://media.railscasts.com/videos/25_sql_injection.mov">http://media.railscasts.com/videos/25_sql_injection.mov</a> 

  <br><a href="http://media.railscasts.com/videos/24_the_stack_trace.mov">http://media.railscasts.com/videos/24_the_stack_trace.mov</a> 

  <br><a href="http://media.railscasts.com/videos/23_counter_cache_column.mov">http://media.railscasts.com/videos/23_counter_cache_column.mov</a> 

  <br><a href="http://media.railscasts.com/videos/22_eager_loading.mov">http://media.railscasts.com/videos/22_eager_loading.mov</a> 

  <br><a href="http://media.railscasts.com/videos/21_super_simple_authentication.mov">http://media.railscasts.com/videos/21_super_simple_authentication.mov</a> 

  <br><a href="http://media.railscasts.com/videos/20_restricting_access.mov">http://media.railscasts.com/videos/20_restricting_access.mov</a> 

  <br><a href="http://media.railscasts.com/videos/19_where_administration_goes.mov">http://media.railscasts.com/videos/19_where_administration_goes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/18_looping_through_flash.mov">http://media.railscasts.com/videos/18_looping_through_flash.mov</a> 

  <br><a href="http://media.railscasts.com/videos/17_habtm_checkboxes.mov">http://media.railscasts.com/videos/17_habtm_checkboxes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/16_virtual_attributes.mov">http://media.railscasts.com/videos/16_virtual_attributes.mov</a> 

  <br><a href="http://media.railscasts.com/videos/15_fun_with_find_conditions.mov">http://media.railscasts.com/videos/15_fun_with_find_conditions.mov</a> 

  <br><a href="http://media.railscasts.com/videos/14_performing_calculations_on_models.mov">http://media.railscasts.com/videos/14_performing_calculations_on_models.mov</a> 

  <br><a href="http://media.railscasts.com/videos/13_dangers_of_model_in_session.mov">http://media.railscasts.com/videos/13_dangers_of_model_in_session.mov</a> 

  <br><a href="http://media.railscasts.com/videos/12_refactoring_user_name_part_3.mov">http://media.railscasts.com/videos/12_refactoring_user_name_part_3.mov</a> 

  <br><a href="http://media.railscasts.com/videos/11_refactoring_user_name_part_2.mov">http://media.railscasts.com/videos/11_refactoring_user_name_part_2.mov</a> 

  <br><a href="http://media.railscasts.com/videos/10_refactoring_user_name_part_1.mov">http://media.railscasts.com/videos/10_refactoring_user_name_part_1.mov</a> 

  <br><a href="http://media.railscasts.com/videos/9_filtering_sensitive_logs.mov">http://media.railscasts.com/videos/9_filtering_sensitive_logs.mov</a> 

  <br><a href="http://media.railscasts.com/videos/8_layouts_and_content_for.mov">http://media.railscasts.com/videos/8_layouts_and_content_for.mov</a> 

  <br><a href="http://media.railscasts.com/videos/7_all_about_layouts.mov">http://media.railscasts.com/videos/7_all_about_layouts.mov</a> 

  <br><a href="http://media.railscasts.com/videos/6_shortcut_blocks_with_symbol_to_proc.mov">http://media.railscasts.com/videos/6_shortcut_blocks_with_symbol_to_proc.mov</a> 

  <br><a href="http://media.railscasts.com/videos/5_using_with_scope.mov">http://media.railscasts.com/videos/5_using_with_scope.mov</a> 

  <br><a href="http://media.railscasts.com/videos/4_move_find_into_model.mov">http://media.railscasts.com/videos/4_move_find_into_model.mov</a> 

  <br><a href="http://media.railscasts.com/videos/3_find_through_association.mov">http://media.railscasts.com/videos/3_find_through_association.mov</a> 

  <br><a href="http://media.railscasts.com/videos/2_dynamic_find_by_methods.mov">http://media.railscasts.com/videos/2_dynamic_find_by_methods.mov</a> 

  <br><a href="http://media.railscasts.com/videos/1_caching_with_instance_variables.mov">http://media.railscasts.com/videos/1_caching_with_instance_variables.mov</a></p>

<p>希望对你有帮助！</p>

<p>呵呵，别把人家服务器下载爆了 :)</p><img src="http://www.cnblogs.com/cnblogsfans/aggbug/1847174.html?type=1" width="1" height="1" alt=""><p>评论: 3　<a href="http://www.cnblogs.com/cnblogsfans/archive/2010/10/10/1847174.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/cnblogsfans/archive/2010/10/10/1847174.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/76832/">技术领域——海量存储计算</a><span style="color:gray">(2010-10-11 08:29)</span><br>· <a href="http://news.cnblogs.com/n/76831/">Google Voice 和 Google Reader 应用确认登陆 WP 7 Marketplace</a><span style="color:gray">(2010-10-11 08:25)</span><br>· <a href="http://news.cnblogs.com/n/76830/">《Angry Birds》开发商抱怨微软乱用其图标</a><span style="color:gray">(2010-10-11 08:21)</span><br>· <a href="http://news.cnblogs.com/n/76829/">暴雪或将于2012年年底推出《魔兽争霸4》</a><span style="color:gray">(2010-10-11 08:17)</span><br>· <a href="http://news.cnblogs.com/n/76828/">微软获得74项Palm智能手机专利授权</a><span style="color:gray">(2010-10-11 08:16)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/cmt/archive/2010/10/10/1847077.html">博客园电子期刊2010年9月刊发布</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>