WP Select Menu
======================

N.B: This repo create just for my personal usage. You may can use it or modify it.

Paste below code locate on functions.php 

```php
class Walker_Nav_Menu_Responsive extends Walker_Nav_Menu {

    // don't output children opening tag ('<ul>')
    public function start_lvl(&$output, $depth=0, $args=array()){}

    // don't output children closing tag    
    public function end_lvl(&$output, $depth=0, $args=array()){}

    public function start_el(&$output, $item, $depth=0, $args=array(), $id = 0){

      // add spacing to the title based on the current depth
      $item->title = str_repeat("&#45; ", $depth ) . $item->title;
      parent::start_el($output, $item, $depth, $args);
	  
	  $output = str_replace( '<a', '<option', $output );
	  $output = str_replace( 'href=', 'value=', $output );

  } 
	
    // replace closing </li> with the closing option tag
    public function end_el(&$output, $item, $depth=0, $args=array()){
	  $output = str_replace( '</a>', '</option>', $output );
    }
}

// Fallback responsive menu when custom header menu has not been set.
function responsivenavfallback() {

	$output = wp_list_pages('echo=0&title_li=');
	$output = str_replace( '<a', '<option', $output );
	$output = str_replace( 'href=', 'value=', $output );
	$output = str_replace( '</a>', '</option>', $output );

	$output = strip_tags( $output, '<div>, <select>, <option>' );

	echo '<div class="select-nav">',
		 '<select onchange="location = this.options[this.selectedIndex].value;"><option value="#">' . __( 'Navigation', 'lan-thinkupthemes') . '</option>' . $output . '</select>',
		 '</div>';
}

```

Use This walker

```php
<div class="select-nav">
    <?php wp_nav_menu( 
      array( 
        'container' => false,
        'theme_location' => 'Menu Location(Locate your registered menu location)',
        'fallback_cb'     => 'responsivenavfallback',
        'items_wrap'     => '<select onchange="location = this.options[this.selectedIndex].value;"><option value="#">' . __( 'Navigation' ) .       '</option>%3$s</select>',
        'walker'         => new Walker_Nav_Menu_Responsive(),  
        'depth'           => 0,
           )
         ); 
    ?>
</div>
```

You can adding any style for select menu: e.g:

```css
// Style
.select-nav select {
    width: 100%;
    color: #FFF;
    background: #505050;
    border: 1px solid #DFE0E4;
    padding: 7px 10px;
    clear: both;
}
```

