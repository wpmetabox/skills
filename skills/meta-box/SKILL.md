---
name: meta-box
description: Write correct code for Meta Box, the WordPress custom fields plugin, and its extensions (MB Custom Table, MB Relationships, MB Settings Page, Meta Box Group, MB User Profile, MB Views, MB Blocks). Use when the project uses Meta Box, when the code calls rwmb_meta(), rwmb_meta_boxes, mb_register_model() or MB_Relationships_API, or when the user asks for custom fields, field groups, relationships, custom tables, custom models, frontend user forms, or Meta Box views.
---

# Meta Box

Meta Box (https://metabox.io) adds custom fields to WordPress. The full docs are at https://docs.metabox.io. The list of common AI mistakes, with correct code for each one, is at https://docs.metabox.io/coding-with-ai/. Read it before you write Meta Box code.

## Rules

1. Use only functions, hooks, and parameters that are in the Meta Box docs. If you cannot find a function in the docs, do not use it. Read the plugin source if the project has it (`wp-content/plugins/meta-box/`).
2. Do not use ACF functions: `get_field()`, `the_field()`, `have_rows()`, `get_sub_field()`. Use `rwmb_meta()` to get a value and `rwmb_the_value()` to output it.
3. Use `rwmb_meta()`, not `get_post_meta()`. Fields with multiple values save one row per value, image and file fields save attachment IDs, and custom table fields are not in post meta.
4. A group saves all its sub-fields in one meta key (the group ID). Get the group, then the sub-field: `( rwmb_meta( 'address' ) ?: [] )['city'] ?? ''`. A cloneable group returns a list of groups.
5. For objects that are not posts, set `object_type` and the object ID:
   - Term: `rwmb_meta( $id, [ 'object_type' => 'term' ], $term_id )`
   - User: `rwmb_meta( $id, [ 'object_type' => 'user' ], $user_id )`
   - Settings page: `rwmb_meta( $id, [ 'object_type' => 'setting' ], $option_name )`
   - Custom model: `rwmb_meta( $id, [ 'object_type' => 'model', 'type' => $model_name ], $model_id )`
   - Custom table: add `'storage_type' => 'custom_table', 'table' => $table`.
6. Relationships are not fields. Use `MB_Relationships_API::get_connected( [ 'id' => $relationship_id, 'from' => $id ] )`, `::add( $from, $to, $id )`, `::delete( $from, $to, $id )`, and `::has( $from, $to, $id )`.
7. Custom models are a feature of MB Custom Table. There is no "Models extension". Register a model with `mb_register_model()` on `init` with priority below 20. `WP_Query` does not work with models: get one row with `\MetaBox\CustomTable\API::get( $id, $table )`, and query many rows with `$wpdb->prepare()` on the model table.
8. The callback of the `rwmb_meta_boxes` filter must return `$meta_boxes`. If it does not, all field groups disappear.
9. Hooks in the docs are shown as `apply_filters( ... )` or `do_action( ... )`. That is the signature. Use `add_filter()` or `add_action()` with the same parameters, and set the number of accepted arguments. A filter callback must return the value.
10. In MB Views (Twig), `{{ post }}` is the current post, also inside the Query Loop block (MB Views 1.15.0+). To show a view in the block editor, use the View block or the `[mbv name="view-name"]` shortcode.

## Extension names

AI tools often invent extension names. Use the correct one:

| Wrong name | Correct |
|---|---|
| Meta Box Models, Models extension | Custom models in MB Custom Table |
| Meta Box Repeater, Repeater field | Meta Box Group with `'clone' => true`, or any field with `'clone' => true` |
| Meta Box Options Page | MB Settings Page |
| Meta Box Relationship field | MB Relationships (not a field) |

## Where to read more

| Task | Docs |
|---|---|
| Register fields with code | https://docs.metabox.io/creating-fields-with-code/ |
| Field settings and field types | https://docs.metabox.io/field-settings/, https://docs.metabox.io/fields/ |
| Get and show field values | https://docs.metabox.io/displaying-fields-with-code/, https://docs.metabox.io/functions/rwmb-meta/ |
| How each field type is saved | https://docs.metabox.io/database/ |
| Cloneable fields and groups | https://docs.metabox.io/cloning-fields/, https://docs.metabox.io/extensions/meta-box-group/ |
| Relationships | https://docs.metabox.io/extensions/mb-relationships/ |
| Custom tables and custom models | https://docs.metabox.io/extensions/mb-custom-table/ |
| Settings pages | https://docs.metabox.io/extensions/mb-settings-page/ |
| Frontend user forms and their hooks | https://docs.metabox.io/extensions/mb-user-profile/ |
| Frontend post forms | https://docs.metabox.io/extensions/mb-frontend-submission/ |
| Views (Twig templates), View block, Query Loop | https://docs.metabox.io/extensions/mb-views/ |
| Gutenberg blocks with fields | https://docs.metabox.io/extensions/mb-blocks/ |
| All actions and filters | https://docs.metabox.io/category/actions/, https://docs.metabox.io/category/filters/ |
| JSON schema for field groups | https://schemas.metabox.io/field-group.json |

## Before you finish

- Check that each function, hook, and parameter in your code is in the docs.
- Escape output (`esc_html()`, `esc_url()`, `esc_attr()`), and use `$wpdb->prepare()` for SQL.
- Tell the user to test the code on a staging site first.
