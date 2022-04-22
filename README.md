# Intro-to-Java
import java.util.*;

public class project {

    public static void main(String[] args) {

        ArrayList <Order> placed_orders = new ArrayList<>();
        HashMap <String, Integer> inventory_sold = new HashMap<>();
        EachItem[] products_list = new EachItem[10];
        double grand_total = 0;

        products_list[0] = new EachItem("red-hot spicy doritos", 2.99);
        products_list[1] = new EachItem("cool ranch doritos", 2.99);
        products_list[2] = new EachItem("coke", 0.99);
        products_list[3] = new EachItem("diet coke", 0.99);
        products_list[4] = new EachItem("pepsi", 0.99);
        products_list[5] = new EachItem("five hour energy", 3.99);
        products_list[6] = new EachItem("sunflower seeds", 0.99);
        products_list[7] = new EachItem("peanuts", 0.99);
        products_list[8] = new EachItem("mac book chargers", 120);
        products_list[9] = new EachItem("dell chargers", 50);

       Scanner input = new Scanner(System.in);

        System.out.println("Allow customers place order. Enter 'closed' when store is closed and customers can no longer place an order");
        System.out.println();

        while (true) {

            System.out.print("Enter your name: ");
            String name = input.nextLine().trim();

            System.out.println("Place orders, when you have finished placing orders, enter quit");
            String nextItem = input.nextLine().toLowerCase().trim();
            double running_total = 0;
            StringBuffer chosen_items = new StringBuffer();

            while(!nextItem.equals("quit")) {

                boolean was_item_found = false;

                for(EachItem item : products_list) {

                    if(nextItem.equals(item.getItem_name())) {
                        running_total += item.getItem_cost();
                        grand_total += running_total;
                        chosen_items.append(item.getItem_name());
                        chosen_items.append("\n");
                        was_item_found = true;
                        inventory_sold.put(item.getItem_name(), inventory_sold.getOrDefault(item.getItem_name(), 0) + 1);
                    }

                }

                if(!was_item_found) System.out.println("Item not sold here. Try another");

                nextItem = input.nextLine().toLowerCase().trim();
            }

            placed_orders.add(new Order(name, chosen_items.toString(), running_total));

            System.out.println("Is store closed? Yes or No.");
            String is_store_closed = input.nextLine().toLowerCase().trim();

            if(!is_store_closed.equals("yes") && !is_store_closed.equals("no")) {
                System.out.println("Enter Yes or No to indicate store availability");

                while (true) {
                    is_store_closed = input.nextLine().toLowerCase().trim();
                    if(!is_store_closed.equals("yes") || !is_store_closed.equals("no")) break;
                }
            }
            if(is_store_closed.equals("yes")) break;
        }


        print_all_orders(placed_orders, inventory_sold, grand_total);
    }

    public static void print_all_orders(ArrayList <Order> placed_orders, HashMap <String, Integer> inventory_sold, double grand_total) {

        for(Order each_order : placed_orders) {
            System.out.println(String.format("name: %s\nOrder Items:\n%s\nTotal: %.2f\n", each_order.getCustomer(), each_order.getItems(), each_order.getTotal()));

        }

        StringBuffer inventory = new StringBuffer();

        for(Map.Entry<String, Integer> map : inventory_sold.entrySet()) {

            inventory.append(map.getKey());
            inventory.append(" ");
            inventory.append("(" + map.getValue() + ")");
            inventory.append("\n");
        }

        System.out.print("Inventory Sold\n " + inventory.toString());
        System.out.print("Grand Total\t" + grand_total);
    }

}


class EachItem {

    String item_name;
    double item_cost;

    public EachItem(String item_name, double item_cost) {
        this.item_name = item_name;
        this.item_cost = item_cost;
    }

    public String getItem_name() {
        return item_name;
    }

    public void setItem_name(String item_name) {
        this.item_name = item_name;
    }

    public double getItem_cost() {
        return item_cost;
    }

    public void setItem_cost(double item_cost) {
        this.item_cost = item_cost;
    }
}

class Order {

    String customer;
    String items;
    double total;

    public Order(String customer, String items, double total) {
        this.customer = customer;
        this.items = items;
        this.total = total;
    }

    public String getCustomer() {
        return customer;
    }

    public void setCustomer(String customer) {
        this.customer = customer;
    }

    public String getItems() {
        return items;
    }

    public void setItems(String items) {
        this.items = items;
    }

    public double getTotal() {
        return total;
    }

    public void setTotal(double total) {
        this.total = total;
    }
}
