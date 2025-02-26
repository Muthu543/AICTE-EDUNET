
# Load the image
image_path = r" "  # Update this with the correct path
img = cv2.imread(image_path)

if img is None:
    print("Error: Could not load image. Check the file path.")
    exit()

# Get image dimensions
height, width, _ = img.shape

msg = input("Enter secret message: ")
password = input("Enter a passcode: ")

# Dictionary to map characters to numbers
d = {chr(i): i for i in range(256)}
c = {i: chr(i) for i in range(256)}

n, m, z = 0, 0, 0

# Encode message into the image
for i in range(len(msg)):
    if n >= height or m >= width:  # Prevent out-of-bounds errors
        print("Error: Message too long for this image size!")
        exit()
    
    img[n, m, z] = d[msg[i]]  # Store ASCII value in pixels
    
    m += 1
    if m >= width:  # Move to the next row if end of width is reached
        m = 0
        n += 1

cv2.imwrite("encryptedImage.jpg", img)
os.system("start encryptedImage.jpg")  # Open the encrypted image

# Decryption process
message = ""
n, m, z = 0, 0, 0

pas = input("Enter passcode for Decryption: ")
if password == pas:
    for i in range(len(msg)):
        if n >= height or m >= width:  # Prevent out-of-bounds errors
            break
        
        message += c[img[n, m, z]]
        
        m += 1
        if m >= width:
            m = 0
            n += 1
    
    print("Decrypted message:", message)
else:
    print("YOU ARE NOT AUTHORIZED!")
