read int
```zig
const std = @import("std");

pub fn main() !void {
    const stdout = std.io.getStdOut();
    const reader = std.io.getStdIn().reader();

    var buffer: [1024]u8 = undefined;
    while (try reader.readUntilDelimiterOrEof(&buffer, '\n')) |line| {
        var number: i32 = try std.fmt.parseInt(i32, line, 10);
        try stdout.writer().print("{d}\n", .{number});
    }
}
```

read lines of string
```zig
const std = @import("std");

fn read_line(reader: anytype, buffer: []u8) !?[]const u8 {
    var line = (try reader.readUntilDelimiterOrEof(
        buffer,
        '\n',
    )) orelse return null;
    return line;
}

test "read until next line" {
    const stdout = std.io.getStdOut();
    const stdin = std.io.getStdIn();

    try stdout.writeAll(
        \\
        \\Enter your name:
    );

    var buffer: [100]u8 = undefined;
    const input = (try read_line(stdin.reader(), &buffer)).?;
    try stdout.writer().print(
        "Your name is: \"{s}\"\n",
        .{input},
    );
}
```

read space separated integers
```zig
const std = @import("std");
const parseInt = std.fmt.parseInt;

test "parse integers" {
    const input = "123 67 89 99";
    const ally = std.testing.allocator;

    var list = std.ArrayList(u32).init(ally);
    // Ensure the list is freed at scope exit.
    // Try commenting out this line!
    defer list.deinit();

    var it = std.mem.tokenize(u8, input, " ");
    while (it.next()) |num| {
        const n = try parseInt(u32, num, 10);
        try list.append(n);
    }

    const expected = [_]u32{ 123, 67, 89, 99 };

    for (expected, list.items) |exp, actual| {
        try std.testing.expectEqual(exp, actual);
    }
}
```

read space separated integers
```zig
const std = @import("std");

pub fn main() !void {
    const stdout = std.io.getStdOut();
    const reader = std.io.getStdIn().reader();
    
    var buffer: [1024]u8 = undefined;
    var line = (try reader.readUntilDelimiterOrEof(&buffer, '\n')).?;
    
    var N = try std.fmt.parseInt(i64, line, 10);
    
    line = (try reader.readUntilDelimiterOrEof(&buffer, '\n')).?;
    
    try stdout.writer().print("{d} {s}\n", .{ N, line });
    
    var arena = std.heap.ArenaAllocator.init(std.heap.page_allocator);
    defer arena.deinit();
    const ally = arena.allocator();
    
    var list = std.ArrayList(u32).init(ally);
    // Ensure the list is freed at scope exit.
    // Try commenting out this line!
    defer list.deinit();
    var it = std.mem.tokenize(u8, line, " ");
    while (it.next()) |num| {
        const n = try std.fmt.parseInt(u32, num, 10);
        try list.append(n);
    }
}
```

read line
```zig
const std = @import("std");

pub fn main() !void {
    const stdout = std.io.getStdOut();
    const reader = std.io.getStdIn().reader();
    var buffer: [1024]u8 = undefined;
    var line = (try reader.readUntilDelimiterOrEof(&buffer, '\n')).?;
    try stdout.writer().print("{s}\n", .{line});
}
```

read int
```zig
const std = @import("std");

pub fn main() !void {
    const stdout = std.io.getStdOut();
    const stdin = std.io.getStdIn();
    const reader = stdin.reader();
    const writer = stdout.writer();
    var buffer: [1024]u8 = undefined;
    var line = (try reader.readUntilDelimiterOrEof(&buffer, '\n')).?;
    var number = try std.fmt.parseInt(i64, line, 10);
    try writer.print("{d}\n", .{number});
}
```

element in an arraylist
```zig
var N = list.items[0]
```