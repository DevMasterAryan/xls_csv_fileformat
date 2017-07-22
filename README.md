# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...

***************************************************
config/application.rb/ 2 lines added

require 'csv'
require 'rails/all'


config/initializer/mime_type added 

Mime::Type.register "application/xls", :xls


# add action in product controller
def index
    @products = Product.all
    respond_to do |format|
      format.html
      format.csv { send_data @products.to_csv }
      format.xls
    end
  end


#product model

 def self.to_csv
    CSV.generate do |csv|
      csv << column_names
      all.each do |product|
        csv << product.attributes.values_at(*column_names)
      end
    end
  end




#view/index.xls.erb



<table border="1">
  <tr>
    <th>ID</th>
    <th>Name</th>
    <th>Price</th>
  </tr>
  <% @products.each do |product| %>
  <tr>
    <td><%= product.id %></td>
    <td><%= product.name %></td>
    
    <td><%= product.price %></td>
  </tr>
  <% end %>
</table>

#view/index.html.erb

<h1>Products</h1>

<p>
  Download:
  <%= link_to "CSV", products_path(format: "csv") %> |
  <%= link_to "Excel", products_path(format: "xls") %>
</p>




****************************************************
references :http://railscasts.com/episodes/362-exporting-csv-and-excel?view=asciicast