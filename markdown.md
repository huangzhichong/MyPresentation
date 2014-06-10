name: inverse
layout: true
class: center, middle, inverse
---
# core_framework refacoring
.footnote[the new core_framework is located [here](https://fndsvn.dev.activenetwork.com/foundation/QA%20Automation/Common/Selenium-Rspec-Experiment/core_framework)]
---
# Why need doing this?
---
name: inverse
layout: true
class: center, middle, inverse
---
# What have been changed?
---
layout: false
.left-column[
  ## Changes
  ### - restructure folders
]
.right-column[
  move files under core_framework to sperate sub-folders:

- `elements` — element_base.rb is moved here, each type of element is put in seperate files.

- `logger` — logger on project level.

- `marquee` — marquee related staff.

- `utilities` — any methods can be shared across projects.

]
---
.left-column[
  ## Changes
  ### - update ElementBase
]
.right-column[
clean the methods in ElementBase

- add new element type `TinyMCE` for rich editor .red[*]

- add methods to `Table` .red[*]
``` remark
get_cell_text_by_row_and_column_number
check_cell_text_by_row_and_column_number
get_header_line_text
check_header_line_text
```
- remove *select* from `DropdownList`, call *select_by_item_text* instead

- rename *select* to *select_by_text* for `MultipleSelect` .red[*]

- rename *input* to *upload_file* for `FileUpload`

- remove *input* from `ElementBase`, put it under `TextInput`

- rename `CheckBox` to `Checkbox`

.footnote[.red.bold[*] the item has not been done]
]
---
.left-column[
  ## Changes
  ### - pdf.rb
]
.right-column[

*pdf.rb* is moved under utilities, the module name is changed to `Utilities::PDF`

```ruby
# old 
Common.pdf_data_check_by_behind_keyword
# replaced with
Utilities::PDF.check_data_by_keyword
```

this method is not used in any project, will need to be deleted.

]
---
.left-column[
  ## Changes
  ### - os.rb
]
.right-column[

*os.rb* is moved under utilities, the module name is changed to `Utilities::OS`

```ruby
# old
Common.os
# replaced with
Utilities::OS.get_current_os
```
]
---
.left-column[
  ## Changes
  ### - database.rb
]
.right-column[
*database.rb* is moved under utilities, the module name is changed to `Utilities::DatabaseUtil`

```ruby
# old
DBConnectionUtil.new
# replaced with
Utilities::DatabaseUtil.new
```
]
---
.left-column[
  ## Changes
  ### - file_utils.rb
]
.right-column[
*file_utils.rb* is renamed to *file_util.rb*, and moved under utilities. the module name is changed to `Utilities::FileUtil`

```ruby
# old
FileUtils.write_xml_file
# replaced with
Utilities::FileUtil.write_xml_file

# old
FileUtils.compare_file_details
# replaced with
Utilities::FileUtil.compare_file_details

# old
FileUtils.compare_files
# replaced with
Utilities::FileUtil.compare_file
```
]

---
.left-column[
  ## Changes
  ### - csv.rb
]
.right-column[

*csv.rb* is moved under utilities, the module name is changed to `Utilities::CSV` 

refactor the way about handling csv files, examples as below,
```ruby       
file = "#{File.dirname(__FILE__)}/report.csv"
csv = Utilities::CSVParser.new(file)
csv.get_data_by_row_and_column_name(1,"Year")
csv.check_row_data_included_by_hash({"Make" => "Chevy"})
csv.get_instance_count_by_row_data_hash({"Make" => "Chevy"})
```
]

---
.left-column[
  ## Changes
  ### - random_profile.rb
]
.right-column[

*random_profile.rb* is merged into *generate_data.rb* under utilities

the module name is changed to `Utilities::GenerateData`
```ruby           
# old
RandomProfile.generate
# replaced with
Utilities::GenerateData.get_user

# old
RandomProfile.generate_audlt
# replaced with
Utilities::GenerateData.get_user_adult

# old
RandomProfile.generate_under_18
# replaced with
Utilities::GenerateData.get_user_child
```
]

---
.left-column[
  ## Changes
  ### - common.rb
]
.right-column[
move Date/Time related methods to *date_time.rb*, added to new module `Utilities::DateTime`
```ruby
Common.get_AgencyNextDay #replaced with 
Utilities::DateTime.get_next_day_by_agency_timezone
Common.convert_local_time_to_timezone_time #replaced with 
Utilities::DateTime.convert_local_time_by_timezone
Common.convert_utc_time_to_timezone_time #replaced with 
Utilities::DateTime.convert_utc_time_by_timezone
Common.convert_local_now_to_timezone_tomorrow #replaced with 
Utilities::DateTime.convert_utc_time_by_timezone
Common.convert_timezone_time_to_utc #replaced with 
Utilities::DateTime.convert_current_local_time_to_next_day_by_timezone
Common.count_age #replaced with 
Utilities::DateTime.get_age_by_birthday
Common.count_dif #replaced with 
Utilities::DateTime.get_second_difference   
Common.format_string_time #replaced with 
Utilities::DateTime.get_time_by_format
Common.two_digits_month #replaced with 
Utilities::DateTime.get_two_digits_month_by_month_name
```
]
---
.left-column[
  ## Changes
  ### - common.rb
]
.right-column[        
move random data generating methods to *generate_data.rb*, the module name is changed to `Utilities::GenerateData`
```ruby
Common.generate_GUID # replaced with
Utilities::GenerateData.get_guid

Common.generate_person_info_by_timestamp # replaced with
Utilities::GenerateData.get_person_info_by_timestamp

Common.get_random_number_string_by_length # replaced with
Utilities::GenerateData.get_number_string_by_length

Common.generate_random_phone_number # replaced with
Utilities::GenerateData.get_phone_number
```        
add new method about generating string by length
```ruby
Utilities::GenerateData.get_string_by_length
```
]
---
.left-column[
  ## Changes
  ### - common.rb
]
.right-column[    
move Base64 encode method to *file_util.rb*
```ruby    
Common.get_base64_encoding_file_content
#replaced with
Utilities::FileUtil.get_base64_encoding_file_content
```

remove *Common.get_file_name_by_time*, this is only used inside respec_example.rb
]
---
.left-column[
  ## Changes
  ### - browser.rb
]
.right-column[
renamed to *web_driver.rb* under utilities, added to new module `WebDriver`
```ruby
Common.restart_browser -> WebDriver.restart_browser 
Common.start_browser -> WebDriver.start_browser 
Common.alert_accept -> WebDriver.alert_accept 
Common.alert_dismiss -> WebDriver.alert_dismiss 
Common.get_alert_content -> WebDriver.get_alert_content 
Common.switch_browser_by_index -> WebDriver.switch_browser_by_index 
Common.switch_to_frame_by_path -> WebDriver.switch_to_frame_by_path 
Common.switch_to_frame_by_name -> WebDriver.switch_to_frame_by_name 
Common.switch_to_frame_by_class -> WebDriver.switch_to_frame_by_class 
Common.switch_to_frame_by_id -> WebDriver.switch_to_frame_by_id 
Common.switch_to_frame_by_xpath -> WebDriver.switch_to_frame_by_xpath 
Common.switch_to_default_frame -> WebDriver.switch_to_default_frame 
Common.refresh_page -> WebDriver.refresh_page 
Common.open_browser_with_url -> WebDriver.navigate_to_url 
Common.navigateTo -> WebDriver.navigate_to_url
Common.get_current_url -> WebDriver.get_current_url 
Common.close_browser -> WebDriver.close_browser 
Common.close_current_window -> WebDriver.close_current_window 
Common.back -> WebDriver.back 
Common.forward -> WebDriver.forward
```
]

---
.left-column[
  ## Changes
  ### - httpwatch.rb
]
.right-column[
*httpwatch.rb* is archived.
]