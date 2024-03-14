# Nforce-Data
Automate your Nforce Data
Below is my code 
def find_index(reel, symbol):
    return [i for i, s in enumerate(reel) if s == symbol]

def create_output_matrix(reels, symbol):
    matrix = []
    for reel in reels:
        indices = find_index(reel, symbol)
        matrix.append(indices)
    return matrix

def subtract_payline(output_matrix, reels, payline):
    for i in range(len(payline)):
        output_matrix[i] = [(index - int(payline[i])) % len(reel) for index, reel in zip(output_matrix[i], reels)]
    return output_matrix

def create_full_matrix(output_matrix):
    max_length = max(len(row) for row in output_matrix)
    full_matrix = []
    for i in range(max_length):
        column = [row[i] if i < len(row) else None for row in output_matrix]
        full_matrix.append(column)
    return full_matrix

def main():
    while True:
        if 'reels' not in locals():
            num_reels = int(input("Enter the number of reels: "))
            reels = [[] for _ in range(num_reels)]

            # Input reels one by one
            for i in range(num_reels):
                reel = input(f"Enter reel {i + 1} (comma-separated symbols): ").split(',')
                reels[i] = reel
        else:
            print("Using previous reels.")
        
        symbol = input("Enter the symbol to search for: ")

        # Create output matrix
        output_matrix = create_output_matrix(reels, symbol)
        base_matrix = [row[:] for row in output_matrix]  # Make a deep copy of output matrix
        print("Output Matrix:")
        for i, row in enumerate(output_matrix):
            print(f"Reel {i + 1}: {row}")

        while True:
            payline = input("Enter the payline (comma-separated numbers): ").split(',')
            output_matrix = [row[:] for row in base_matrix]  # Revert to the base matrix
            output_matrix = subtract_payline(output_matrix, reels, payline)

            # Transpose the output matrix
            full_matrix = create_full_matrix(output_matrix)
            print("Full Matrix:")
            for row in full_matrix:
                print(row)

            choice = input("Do you want to enter another payline? (yes/no): ")
            if choice.lower() != 'yes':
                break

        choice = input("Do you want to enter another symbol? (yes/no): ")
        if choice.lower() != 'yes':
            break

if __name__ == "__main__":
    main()
