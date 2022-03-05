1. [System](README_system.md) (white)
1. [Request](README_request.md) (blue)
1. [Accept](README_accept.md) (green)
1. [Retrieve](README_retrieve.md) (white)
1. [Precondition](README_precondition.md) (yellow)
1. Create/Process
    * [Create](README_create.md) (violet)
    * [Process](README_process.md) (red)
1. [Response](README_response.md) (cyan)
1. [Alternative](README_alternative.md) (gray)

![HTTP headers status](https://rawgithub.com/for-GET/http-decision-diagram/master/httpdd.png)

## Create

| | callback | output | default
|:-- | ---: | :--- | :---
|K3 | [`method :var`](#method-var) | *Method* | `Transaction.request.method`
| | [`is_method_put : in`](#is_method_put--in) | T / F |
| | [`create_is_method_put : in`](#create_is_method_put--in) | T / F | `is_method_put : in`
|L3 | [`create_partial_put :bin`](#create_partial_put-bin) | T / F | `process_partial_put :bin`
|M3 | [`create_has_conflict :bin`](#create_has_conflict-bin) | T / F | `process_has_conflict :bin`
|K2 | [`method :var`](#method-var) | *Method* | `Transaction.request.method`
| | [`create_methods :var`](#create_methods-var) | [ *Method* ] | [ POST<br>, PUT<br>, PATCH<br>]
| | [`is_method_create : in`](#is_method_create--in) | T / F |
|L2 | [`path :var`](#path-var) | *URI* |
| | [`create_path : in`](#create_path--in) | T / F |
|M2 | [`create :bin`](#create-bin) | T / F | FALSE

> FIXME Explanations needed
