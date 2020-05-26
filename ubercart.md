# Ubercart

When switching between live/sandbox environment, make sure to change both config of

* Paypal Website Payment Standard under payment method
* Paypal Website Payment Pro under payment gateway.

Ubercart paypal populate payment method callback based on the existence of `$_SESSION['TOKEN']`

```php
<?php
/**
 * Allow a customer to review their order before finally submitting it.
 *
 * @see uc_cart_checkout_form()
 */
function uc_cart_checkout_review() {
  drupal_add_js(drupal_get_path('module', 'uc_cart') .'/uc_cart.js');
  $form = drupal_get_form('uc_cart_checkout_review_form');

  if ($_SESSION['do_review'] !== TRUE && !uc_referer_check('cart/checkout')) {
    drupal_goto('cart/checkout');
  }
?>
```

Payment callback registered under \#submit handler through hook\_form\_alter of payment module using this form\_id.

