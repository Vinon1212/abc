class Room:
    def __init__(self, room_number, capacity, price):
        self.room_number = room_number
        self.capacity = capacity
        self.price = price
        self.is_reserved = False

    def reserve(self):
        self.is_reserved = True

    def cancel_reservation(self):
        self.is_reserved = False


class Hotel:
    def __init__(self, name):
        self.name = name
        self.rooms = {}

    def add_room(self, room_number, capacity, price):
        if room_number not in self.rooms:
            room = Room(room_number, capacity, price)
            self.rooms[room_number] = room

    def remove_room(self, room_number):
        if room_number in self.rooms:
            del self.rooms[room_number]

    def get_available_rooms(self):
        available_rooms = []
        for room_number, room in self.rooms.items():
            if not room.is_reserved:
                available_rooms.append(room)
        return available_rooms


class Reservation:
    def __init__(self, guest_name, room_number, check_in_date, check_out_date):
        self.guest_name = guest_name
        self.room_number = room_number
        self.check_in_date = check_in_date
        self.check_out_date = check_out_date


class ReservationSystem:
    def __init__(self, hotel):
        self.hotel = hotel
        self.reservations = []

    def make_reservation(self, guest_name, room_number, check_in_date, check_out_date):
        room = self.hotel.rooms.get(room_number)
        if room and not room.is_reserved:
            room.reserve()
            reservation = Reservation(guest_name, room_number, check_in_date, check_out_date)
            self.reservations.append(reservation)
            return True
        return False

    def cancel_reservation(self, guest_name):
        for reservation in self.reservations:
            if reservation.guest_name == guest_name:
                room = self.hotel.rooms.get(reservation.room_number)
                if room:
                    room.cancel_reservation()
                self.reservations.remove(reservation)
                return True
        return False


# Example usage
hotel = Hotel("Sample Hotel")
hotel.add_room(101, 2, 100)
hotel.add_room(102, 4, 200)
hotel.add_room(103, 1, 50)

reservation_system = ReservationSystem(hotel)

# Make a reservation
reservation_made = reservation_system.make_reservation("John Doe", 101, "2023-05-25", "2023-05-28")
if reservation_made:
    print("Reservation successful!")
else:
    print("Room not available.")

# Cancel a reservation
reservation_canceled = reservation_system.cancel_reservation("John Doe")
if reservation_canceled:
    print("Reservation canceled.")
else:
    print("Reservation not found.")

# Get available rooms
available_rooms = hotel.get_available_rooms()
print("Available rooms:")
for room in available_rooms:
    print(f"Room {room.room_number}, Capacity: {room.capacity}, Price: {room.price}")

