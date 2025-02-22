# Product Stock Tracker - Visual programming project

## Application Description
The goal of the project is to manage product sales and inventory information using the access database. The program will generally include products, categories and sales.The project also has additional features such as listening to the radio.

### ER Diagram (Microsoft Access)
![ER Diagram](https://github.com/ceydahuseini/product_stock_tracker/blob/e703be595f6fdf758e29dc502915bf9b55be16d1/project_images/er_diagram.png)

## Usage
Let's examine the admin panel that allows us to log in to the program. The image you see below is the program's admin login panel. The username is "user" and the password is "1234." If the username and password are entered correctly, the program will log into the system; otherwise, the program will give an error and will not log into the system.

![Admin Picture](https://github.com/ceydahuseini/product_stock_tracker/blob/5aa93d2a47e4ec511a6f32c0ce51d44bed82f1af/project_images/1.png)
![Admin Picture](https://github.com/ceydahuseini/product_stock_tracker/blob/07e1b3d05f3beba43250cb8623d2ab45366eba87/project_images/2.png)

### Change password
There is also a password change area in the application. You can change your password by clicking on the change password button. There is only one username in the system, which is "user". After entering the current password, if you enter and confirm the new password, the password will change. You can see all the situations in the images below.

![Change Passoword](https://github.com/ceydahuseini/product_stock_tracker/blob/07e1b3d05f3beba43250cb8623d2ab45366eba87/project_images/3.png)
![Change Passoword](https://github.com/ceydahuseini/product_stock_tracker/blob/07e1b3d05f3beba43250cb8623d2ab45366eba87/project_images/4.png)
![Change Passoword](https://github.com/ceydahuseini/product_stock_tracker/blob/07e1b3d05f3beba43250cb8623d2ab45366eba87/project_images/5.png)

### Home Page
When we log into the system correctly, we go to the home page. The project consists of a total of six parts. Let us examine these six sections in detail.

![Home Page](https://github.com/ceydahuseini/product_stock_tracker/blob/c49524cfc7d1d1e375a42509ff56aea4926ea16c/project_images/6.png)


### Categories
Adding, updating and deleting categories to the database can be done in the system. Additionally, there is a clear button since the selected category name is transferred to the textbox. You can clear the textbox with Clear. The use of all these operations is given in the video below.

[Watch Categories Video](https://drive.google.com/file/d/1QNsyAphnLe-ZRZallUmuFf_oQmWlVgQT/view?usp=sharing)

### Products
In the product form, there are also add, update, delete and clear buttons. Let's briefly examine the columns in the list view.
1) Product Name: Briefly represents the name of the product
2) Description: More detailed information or explanation about the product
3) Price: The price of the product
4) Stock Quantity: Stock represents how many of that product are left.
5) Brand: Brand of the product
6) Category: Column showing which category the product belongs to.

Meanwhile, all possible mistakes that the user can make are taken into account. For example, trying to delete a product without selecting it, entering a negative quantity, entering empty information, etc.

[Watch Products Video](https://drive.google.com/file/d/1PVVK8oDBhOPhBGid3HWXXGz5DSkEObDc/view?usp=sharing)

### Sales
In the sale form, there are sale, update, delete, clear and calculate buttons.
The Sale button is used to make sales. In order for the sale to be made, the product must be in stock, otherwise you will receive a warning message that the product is not in stock. Again, you cannot make a sale at a later date. If you try to make a sale, you will receive a warning message (Errorprovider).

When you make a sale, the number of products in stock is also updated.
For example, if there are 10 phones in stock, if you sell 3, the number of products in stock will be 7.

Update is the button used to update sales.
Delete is the button used to delete the sale.
Clear is the button used to clear the information of the selected product from tools (texbox, combox, etc.).
The Calculate button is the button that only helps you find out the total price. It finds the total price by multiplying the quantity and price in the background. You can also sell without using the Calculate button, but I recommend you use it so that you can find out the price of the product without going to the Product form and do not calculate it manually when selling.

Meanwhile, all possible mistakes that the user can make are taken into consideration. For example, trying to delete without selecting sale, entering negative quantity, entering empty information, etc. The project also includes the necessary erroproviders.

[Watch Sales Video](https://drive.google.com/file/d/1Ooe3-zLm36t8en7_CINzeyUCeQlttqmK/view?usp=sharing)


### Radio
To listen to the radio, you need to click on the button to listen to the radio. There are 4 channels in the form of radio in total, you can click on the channel you want and listen to it.

<b>You must have an internet connection to listen to the radio.</b>

![Radio](https://github.com/ceydahuseini/product_stock_tracker/blob/61a6d913ab798d65d4aca892101ec3a4e8f3d3f0/project_images/7.png)

### Login Page
It redirects you back to the login page.

![About](https://github.com/ceydahuseini/product_stock_tracker/blob/3251edf0c9315d1193b17a2a4e02cd8eb65025ba/project_images/9.png)

### About
A short message about me and my project.
![About](https://github.com/ceydahuseini/product_stock_tracker/blob/61a6d913ab798d65d4aca892101ec3a4e8f3d3f0/project_images/8.png)

## Description of the Sales Form(FrmSale Form)
The project has many important functions, but in my opinion, the most important one is that we need to consider certain parameters when making sales in the sales form. Such as the existence of the product in stock, not entering a future date, not entering negative quantity, and updating the number of products in stock when making a sale.
The `private bool IsStockAvailable(string productId, int requestedQuantity)` method is one of the important methods.

The IsStockAvailable method checks if there's enough product in stock. It connects to the database, retrieves the current stock level for a given product ID, and compares it to the requested amount. If there's enough stock, it returns true, otherwise it returns false. It also handles errors during database interaction for reliability.

        private bool IsStockAvailable(string productId, int requestedQuantity)
        {
            try
            {
                using (OleDbConnection oleDbConnection = new OleDbConnection(connectionString))
                {
                    oleDbConnection.Open();
                    OleDbCommand command = new OleDbCommand("SELECT StockQuantity FROM Product WHERE ProductID = @ProductID", oleDbConnection);
                    command.Parameters.AddWithValue("@ProductID", productId);
                    int currentStock = (int)command.ExecuteScalar();

                    return currentStock >= requestedQuantity;
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("Error checking stock availability: " + ex.Message);
                return false;
            }
        }

UpdateStockQuantity is the method used to update the number of products in stock when a sale is made. Similarly, when the sale is updated, the same method updates the number of products in stock. When a sale is made, the number of products in stock decreases and when an update is made or a return is made, the number of products in stock increases.

     private void UpdateStockQuantity(string productId, int quantityDifference)
        {
            try
            {
                using (OleDbConnection oleDbConnection = new OleDbConnection(connectionString))
                {
                    oleDbConnection.Open();
                    OleDbCommand command = new OleDbCommand("UPDATE Product SET StockQuantity = StockQuantity + @QuantityDifference WHERE ProductID = @ProductID", oleDbConnection);
                    command.Parameters.AddWithValue("@QuantityDifference", quantityDifference);
                    command.Parameters.AddWithValue("@ProductID", productId);
                    command.ExecuteNonQuery();
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("Error updating stock quantity: " + ex.Message);
            }
        }

