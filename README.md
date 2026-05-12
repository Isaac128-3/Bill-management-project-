# Bill-management-project-

use std::collections::HashMap;
use std::io;

fn main() {
    let mut bills: HashMap<String, f64> = HashMap::new();

    loop {
        println!("\n===== BILL MANAGER =====");
        println!("1. Add Bill");
        println!("2. View Bills");
        println!("3. Remove Bill");
        println!("4. Edit Bill");
        println!("5. Exit");

        let choice = get_input("Choose an option: ");

        match choice.trim() {
            "1" => add_bill(&mut bills),
            "2" => view_bills(&bills),
            "3" => remove_bill(&mut bills),
            "4" => edit_bill(&mut bills),
            "5" => {
                println!("Goodbye!");
                break;
            }
            _ => println!("Invalid choice."),
        }
    }
}

fn get_input(prompt: &str) -> String {
    let mut input = String::new();
    println!("{}", prompt);

    io::stdin()
        .read_line(&mut input)
        .expect("Failed to read input");

    input.trim().to_string()
}

fn add_bill(bills: &mut HashMap<String, f64>) {
    let name = get_input("Enter bill name:");
    let amount = get_input("Enter amount:");

    match amount.parse::<f64>() {
        Ok(value) => {
            bills.insert(name.clone(), value);
            println!("Added '{}' - ${:.2}", name, value);
        }
        Err(_) => println!("Invalid amount."),
    }
}

fn view_bills(bills: &HashMap<String, f64>) {
    if bills.is_empty() {
        println!("No bills found.");
    } else {
        println!("\n--- Bills ---");
        for (name, amount) in bills {
            println!("{} : ${:.2}", name, amount);
        }
    }
}

fn remove_bill(bills: &mut HashMap<String, f64>) {
    let name = get_input("Enter bill name to remove:");

    if bills.remove(&name).is_some() {
        println!("Bill removed.");
    } else {
        println!("Bill not found.");
    }
}

fn edit_bill(bills: &mut HashMap<String, f64>) {
    let name = get_input("Enter bill name to edit:");

    if bills.contains_key(&name) {
        println!("Type 'back' to cancel.");

        let new_amount = get_input("Enter new amount:");

        if new_amount.to_lowercase() == "back" {
            println!("Edit cancelled.");
            return;
        }

        match new_amount.parse::<f64>() {
            Ok(value) => {
                bills.insert(name.clone(), value);
                println!("Updated '{}' to ${:.2}", name, value);
            }
            Err(_) => println!("Invalid amount."),
        }
    } else {
        println!("Bill not found.");
    }
}