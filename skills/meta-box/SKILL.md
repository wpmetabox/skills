---
name: meta-box
description: Rules to write correct code for Meta Box, the WordPress custom fields plugin, and its extensions (MB Custom Table, MB Relationships, MB Settings Page, Meta Box Group, MB User Profile, MB Views). Also lists the mistakes that AI tools make most often, with the correct code. Use when the project uses Meta Box, when the code calls rwmb_meta(), rwmb_meta_boxes, mb_register_model() or MB_Relationships_API, or when the user asks for custom fields, relationships, custom tables, custom models, user forms, or views with Meta Box.
---

# Meta Box

This skill gives the rules for Meta Box code. It also lists the mistakes that AI tools make most often. Code examples are at https://docs.metabox.io/coding-with-ai/. The full docs are at https://docs.metabox.io.

## How to write Meta Box code

### Create fields

- Register the field group with the `rwmb_meta_boxes` filter.
- Or save the field group as a JSON file. Use the schema https://schemas.metabox.io/field-group.json to check the settings.
- Or create the field group in MB Builder. To get the PHP code, click **Get PHP Code**.
- Use a prefix for field IDs, for example `event_date`.
- Use only the field types and settings in the docs.

### Get field values

- `rwmb_meta()` gets a value. `rwmb_the_value()` shows a value.
- For a term, user, settings page, or custom model, set `object_type`.
- To show fields without PHP, use MB Views, Block Bindings, or a page builder.

### Select the correct extension

| Task | Extension |
|---|---|
| Repeat a set of fields | Meta Box Group with `'clone' => true` |
| Connect posts, terms, or users | MB Relationships |
| Save fields in a custom table | MB Custom Table |
| Save data that is not a post (orders, logs) | Custom models in MB Custom Table |
| Make an options page | MB Settings Page |
| Make a registration, login, or profile form | MB User Profile |
| Make a form to submit posts | MB Frontend Submission |
| Make a Gutenberg block with fields | MB Blocks |
| Show or hide fields with conditions | Meta Box Conditional Logic |
| Make templates without PHP | MB Views |

### Write safe code

- Escape output with `esc_html()`, `esc_url()`, or `esc_attr()`.
- Use `$wpdb->prepare()` for SQL.

### If you are not sure

- Read the docs (see the table at the end).
- If the project has the plugin, read the source in `wp-content/plugins/meta-box/`.
- Do not use a function, hook, or parameter that is not in the docs or in the source.

## Common mistakes

| Mistake | Correct |
|---|---|
| ACF functions: `get_field()`, `the_field()`, `have_rows()`, `get_sub_field()` | `rwmb_meta()` gets a value. `rwmb_the_value()` shows a value. |
| `get_post_meta()` | `rwmb_meta()`. Fields with multiple values save one row for each value. Image fields save IDs. Custom table fields are not in post meta. |
| A sub-field ID as a meta key: `rwmb_meta( 'city' )` | Get the group, then the sub-field: `( rwmb_meta( 'address' ) ?: [] )['city'] ?? ''`. A cloneable group gives a list of groups. |
| No `object_type` for a term, user, settings page, or model | `rwmb_meta( $id, [ 'object_type' => 'term' ], $term_id )`. Use `'user'`, `'setting'` (3rd parameter is the option name), or `'model'` with `'type' => $model_name`. For a custom table, set `'storage_type' => 'custom_table', 'table' => $table`. |
| A relationship as a field | `MB_Relationships_API::get_connected( [ 'id' => $relationship_id, 'from' => $id ] )`, `::add( $from, $to, $id )`, `::delete( $from, $to, $id )`, `::has( $from, $to, $id )`. |
| "Models extension" | Custom models are a feature of MB Custom Table. |
| `WP_Query` or `get_posts()` for custom models | One row: `\MetaBox\CustomTable\API::get( $id, $table )`. Many rows: `$wpdb->prepare()` on the model table. |
| `mb_register_model()` on `init` with priority 20 or higher | Use priority 10 (the default). With priority 20 or higher, the model fields do not work. |
| "Meta Box Repeater" | Meta Box Group with `'clone' => true`. |
| "Meta Box Options Page" | MB Settings Page. |
| `apply_filters()` or `do_action()` copied from the docs | These are signatures. Use `add_filter()` or `add_action()`. Set the number of parameters if there is more than 1. A filter callback must return the value. |
| No `return $meta_boxes` in the `rwmb_meta_boxes` callback | Return `$meta_boxes`. If not, all field groups disappear. |
| "MB Views does not work in the Query Loop block, use ACF" | In MB Views 1.15.0 and later, `{{ post }}` is the current post of the loop. Add the View block or `[mbv name="view-name"]` in the Post Template block. |

## Docs

| Topic | URL |
|---|---|
| Register fields with code | https://docs.metabox.io/creating-fields-with-code/ |
| Field settings, field types | https://docs.metabox.io/field-settings/, https://docs.metabox.io/fields/ |
| Get field values | https://docs.metabox.io/displaying-fields-with-code/, https://docs.metabox.io/functions/rwmb-meta/ |
| Database | https://docs.metabox.io/database/ |
| Clone, groups | https://docs.metabox.io/cloning-fields/, https://docs.metabox.io/extensions/meta-box-group/ |
| Relationships | https://docs.metabox.io/extensions/mb-relationships/ |
| Custom tables, custom models | https://docs.metabox.io/extensions/mb-custom-table/ |
| Settings pages | https://docs.metabox.io/extensions/mb-settings-page/ |
| User forms and hooks | https://docs.metabox.io/extensions/mb-user-profile/ |
| Post forms | https://docs.metabox.io/extensions/mb-frontend-submission/ |
| Views, View block, Query Loop | https://docs.metabox.io/extensions/mb-views/ |
| Blocks | https://docs.metabox.io/extensions/mb-blocks/ |
| Actions, filters | https://docs.metabox.io/category/actions/, https://docs.metabox.io/category/filters/ |
| JSON schema | https://schemas.metabox.io/field-group.json |
