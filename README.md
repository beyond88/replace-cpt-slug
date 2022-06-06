# replace-cpt-slug

WordPress filter trick to change slug of a Custom Post Type 

```
function add_custom_rewrite_rule() {

    // First, try to load up the rewrite rules. We do this just in case
    // the default permalink structure is being used.
    if( ($current_rules = get_option('rewrite_rules')) ) {

        // Next, iterate through each custom rule adding a new rule
        // that replaces 'movies' with 'films' and give it a higher
        // priority than the existing rule.
        foreach($current_rules as $key => $val) {
            if(strpos($key, 'projects') !== false) {
                add_rewrite_rule(str_ireplace('projects', 'prosjekter', $key), $val, 'top');   
            } // end if
        } // end foreach

    } // end if/else

    // ...and we flush the rules
    flush_rewrite_rules();

} // end add_custom_rewrite_rule
add_action('init', 'add_custom_rewrite_rule');

function change_post_types_slug( $args, $post_type ) {

   /*item post type slug*/   
   if ( 'projects' === $post_type ) {
      $args['rewrite']['slug'] = 'prosjekter';
   }

   return $args;
}
add_filter( 'register_post_type_args', 'change_post_types_slug', 10, 2 );```
