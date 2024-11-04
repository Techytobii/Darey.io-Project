#!/bin/bash

# Function to display the multiplication table in a specified format
display_multiplication_table() {
    local number=$1
    local start=$2
    local end=$3
    local format=$4

    echo -e "\nMultiplication Table for $number from $start to $end:"

    for (( i=start; i<=end; i++ )); do
        if [[ "$format" == "1" ]]; then
            # Standard format
            printf "%d x %d = %d\n" "$number" "$i" "$(( number * i ))"
        elif [[ "$format" == "2" ]]; then
            # Fancy format
            printf "%d x %d = %d\n" "$number" "$i" "$(( number * i ))" | awk '{printf "%-10s", $0; if(NR%5==0) print ""}'
        elif [[ "$format" == "3" ]]; then
            # Vertical format
            echo "$number x $i = $(( number * i ))"
        fi
    done
    echo ""
}

# Main script execution
while true; do
    # Prompt the user to enter a number
    while true; do
        read -p "Enter a number to see its multiplication table: " number
        if [[ "$number" =~ ^-?[0-9]+$ ]]; then
            break  # Exit the loop if input is valid
        else
            echo "Invalid input. Please enter a valid integer."
        fi
    done

    # Ask the user for their preference on the multiplication table
    read -p "Do you want to see a full table (1-10) or a partial table (y/n)? " choice

    if [[ "$choice" == "y" || "$choice" == "Y" ]]; then
        # Ask for the range for the partial table
        while true; do
            read -p "Enter the starting number for the range (1-10): " start
            read -p "Enter the ending number for the range (1-10): " end
            
            # Validate the range
            if [[ "$start" =~ ^[1-9]$ || "$start" == "10" ]] && [[ "$end" =~ ^[1-9]$ || "$end" == "10" ]] && [ "$start" -le "$end" ]; then
                break  # Exit the loop if the range is valid
            else
                echo "Invalid range. Please enter a valid range between 1 and 10."
            fi
        done

    else
        # Default to full multiplication table from 1 to 10
        start=1
        end=10
    fi

    # Ask for display format
    echo "Choose a display format:"
    echo "1. Standard"
    echo "2. Fancy (5 results per line)"
    echo "3. Vertical"
    read -p "Enter your choice (1-3): " format

    # Validate format choice
    while [[ "$format" -lt 1 || "$format" -gt 3 ]]; do
        echo "Invalid choice. Please select a format between 1 and 3."
        read -p "Enter your choice (1-3): " format
    done

    # Display the multiplication table for the specified range
    display_multiplication_table "$number" "$start" "$end" "$format"

    # Ask if the user wants to repeat the process
    read -p "Do you want to generate another multiplication table? (y/n): " repeat
    if [[ "$repeat" != "y" && "$repeat" != "Y" ]]; then
        echo "Thank you for using the multiplication table generator!"
        break
    fi
done


