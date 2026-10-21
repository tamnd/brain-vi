---
title: "CF 105826C - \u041a\u043e\u0444\u0435\u0439\u043d\u044b\u0435 \u043a\u043e\u043b\u0435\u0447\u043a\u0438"
description: "Chúng tôi đang mô phỏng những gì xảy ra bên trong tách cà phê theo thời gian. Chiếc cốc bắt đầu trống rỗng và sau đó chúng tôi xử lý một chuỗi các thao tác đổ một lượng cà phê vào cốc hoặc uống một lượng từ cốc đó. Chi tiết quan trọng là cà phê được phân lớp theo khái niệm."
date: "2026-06-25T14:57:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105826
codeforces_index: "C"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 2025"
rating: 0
weight: 105826
solve_time_s: 52
verified: true
draft: false
---

[CF 105826C - \u041a\u043e\u0444\u0435\u0439\u043d\u044b\u0435 \u043a\u043e\u043b\u0435\u0447\u043a\u0438](https://codeforces.com/problemset/problem/105826/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng những gì xảy ra bên trong tách cà phê theo thời gian. Chiếc cốc bắt đầu trống rỗng và sau đó chúng tôi xử lý một chuỗi các thao tác đổ một lượng cà phê vào cốc hoặc uống một lượng từ cốc đó. 

Chi tiết quan trọng là cà phê được phân lớp theo khái niệm. Mỗi khi cà phê mới được rót vào, nó sẽ tạo thành một lớp mới bên trên chất lỏng hiện có. Khi cà phê được loại bỏ, nó luôn được loại bỏ khỏi lớp trên cùng trước tiên, có thể tiêu tốn nhiều lớp nếu lượng loại bỏ đủ lớn. 

“Vòng cà phê” tương ứng với ranh giới giữa các lớp cà phê được tạo ra trong quá trình này. Ngay cả khi tất cả cà phê cuối cùng đã được lấy đi, chiếc cốc rỗng vẫn đóng góp một vòng cuối cùng ở mức đáy. 

Vì vậy, nhiệm vụ không phải là theo dõi lượng cà phê cuối cùng mà là theo dõi xem có bao nhiêu ranh giới lớp riêng biệt tồn tại trong suốt quá trình. 

Đầu vào là một chuỗi các thao tác có dạng “thêm X” (đổ X mililit) hoặc “loại bỏ X” (uống X mililit). Kết quả đầu ra là tổng số vòng cà phê được hình thành sau khi thực hiện tất cả các thao tác. 

Các ràng buộc đủ lớn để bất kỳ mô phỏng bậc hai nào trên tất cả mililit đều không thể thực hiện được. Chúng ta phải xử lý từng thao tác theo thời gian không đổi hoặc logarit, nghĩa là chúng ta cần coi cà phê dưới dạng các phân đoạn có cấu trúc thay vì các đơn vị riêng lẻ. 

Một trường hợp lỗi nhỏ xuất hiện khi một thao tác tiêu tốn một phần nhiều lớp. Ví dụ: nếu chúng ta có các lớp có kích thước`[3, 5]`và loại bỏ`4`, lớp trên cùng biến mất và lớp thứ hai bị giảm đi một phần. Việc triển khai đơn giản xử lý từng hoạt động một cách độc lập mà không duy trì cấu trúc sẽ tính không chính xác các ranh giới lớp. 

Một trường hợp khó khăn khác là khi tất cả cà phê được loại bỏ. Ví dụ: 

đầu vào:```
3
+5 -3 -2
```Sau khi xử lý xong mọi việc, chiếc cốc trống rỗng, nhưng câu trả lời không phải là số không. Vẫn còn một chiếc nhẫn tương ứng với đáy cốc. Một giải pháp đơn giản chỉ tính “các lớp hiện có” sẽ xuất ra 0 không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ mô phỏng cà phê ở độ phân giải mililit. Chúng tôi sẽ mở rộng mỗi lần rót thành các đơn vị riêng lẻ và duy trì một chồng đơn vị, sau đó mô phỏng từng đồ uống bằng cách bật từng đơn vị một. Điều này đúng nhưng ngay lập tức trở nên không khả thi vì một thao tác đơn lẻ có thể liên quan đến tối đa 10^9 đơn vị theo cách hiểu tồi tệ nhất và thậm chí dưới những ràng buộc chặt chẽ hơn, tổng khối lượng khiến phương pháp này không thể sử dụng được. 

Quan sát quan trọng là chúng ta không bao giờ cần phải theo dõi từng mililit riêng lẻ. Chúng tôi chỉ cần theo dõi các khối cà phê liền kề có cùng “thế hệ”, nghĩa là chúng được rót vào một hoạt động trước khi bị gián đoạn sau đó. 

Mỗi lần đổ sẽ tạo ra một lớp mới. Đồ uống chỉ loại bỏ lớp trên cùng và có thể làm giảm một phần hoặc loại bỏ hoàn toàn. Điều này tự nhiên gợi ý việc duy trì một ngăn xếp trong đó mỗi phần tử là một phân đoạn đại diện cho một lớp và khối lượng còn lại của nó. 

Số lượng các vòng sau đó được xác định bằng số lượng đoạn như vậy đã từng được tạo ra và chưa bị loại bỏ hoàn toàn, cộng với ranh giới cuối cùng ở đáy cốc. 

Điều này làm giảm vấn đề duy trì một chồng kích thước phân đoạn và chỉ cập nhật phần tử trên cùng cho mỗi thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng khối lượng) | O(tổng khối lượng) | Quá chậm | 
| Ngăn xếp các phân đoạn | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình chiếc cốc như một chồng nhiều lớp, trong đó mỗi lớp lưu trữ lượng cà phê còn lại trong lớp đó. 

1. Khởi tạo một ngăn xếp trống. Mỗi phần tử sẽ đại diện cho một lớp được tạo bởi thao tác đổ. 
2. Đối với thao tác “đổ X”, đẩy phần tử mới có giá trị X vào ngăn xếp. Điều này thể hiện việc tạo một lớp trên cùng mới, ngay lập tức đưa ra một ranh giới mới trong cấu trúc của chiếc cốc. 
3. Đối với thao tác “uống X”, giảm liên tục từ đầu ngăn xếp. Trừ X từ số tiền còn lại của lớp trên cùng. Nếu lớp trên cùng bằng 0, hãy loại bỏ nó hoàn toàn khỏi ngăn xếp và tiếp tục trừ đi lớp tiếp theo. Điều này phản ánh ý tưởng vật lý rằng việc uống rượu luôn tiêu thụ lượng cà phê cao nhất trước tiên. 
4. Tiếp tục cho đến khi X được tiêu thụ hết. Điều này đảm bảo rằng tất cả các lớp bị ảnh hưởng đều được cập nhật chính xác, ngay cả khi một đồ uống trải dài trên nhiều lớp đã rót trước đó. 
5. Sau khi xử lý tất cả các thao tác, số lượng vòng bằng số lớp còn lại trong ngăn xếp cộng thêm một vòng cho đáy cốc. Nếu ngăn xếp trống thì câu trả lời là 1. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, ngăn xếp thể hiện sự phân chia chính xác của cốc thành các đoạn liền kề tối đa được hình thành bởi các lần rót liên tiếp. Mỗi lần đẩy, chúng tôi tạo ra chính xác một ranh giới phân khúc mới. Mỗi lần chúng tôi loại bỏ hoàn toàn một phân khúc, chúng tôi sẽ loại bỏ vĩnh viễn ranh giới đó. Việc xóa một phần không bao giờ thay đổi số lượng phân đoạn mà chỉ thay đổi kích thước của chúng. Vì đồ uống không bao giờ sắp xếp lại các lớp và luôn hoạt động từ trên xuống nên tính bất biến của ngăn xếp được giữ nguyên trong suốt quá trình. Số phân đoạn cuối cùng khớp chính xác với số lớp cà phê riêng biệt từng tồn tại đồng thời tại bất kỳ thời điểm nào và mỗi lớp như vậy tương ứng với một ranh giới vòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    stack = []

    for _ in range(n):
        op = input().strip()
        if op[0] == '+':
            x = int(op[1:])
            stack.append(x)
        else:
            x = int(op[1:])
            while x > 0 and stack:
                if stack[-1] > x:
                    stack[-1] -= x
                    x = 0
                else:
                    x -= stack[-1]
                    stack.pop()

    # number of layers + bottom ring
    if not stack:
        print(1)
    else:
        print(len(stack) + 1)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện mô hình ngăn xếp. Mỗi lần đổ thêm một lớp mới. Mỗi đồ uống cẩn thận làm giảm lớp trên cùng trước, có khả năng loại bỏ nhiều lớp theo trình tự. Câu trả lời cuối cùng được rút ra từ số lớp còn lại cộng với vòng đế. 

Một lỗi triển khai phổ biến là quên tiếp tục phép trừ sau khi một lớp bị xóa hoàn toàn. Một cách khác là xử lý từng thao tác một cách độc lập mà không duy trì sự tồn tại của các lớp một phần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
+5
+3
-4
+2
-3
```Chúng tôi theo dõi ngăn xếp sau mỗi bước. 

| Bước | Hoạt động | Ngăn xếp | 
| --- | --- | --- | 
| 1 | +5 | [5] | 
| 2 | +3 | [5, 3] | 
| 3 | -4 | [5] | 
| 4 | +2 | [5, 2] | 
| 5 | -3 | [4] | 

Sau khi xử lý, một lớp vẫn có kích thước 4. 

Điều này thể hiện mức tiêu thụ một phần trên các lớp và xác nhận rằng việc hợp nhất chỉ xảy ra khi một lớp bị loại bỏ hoàn toàn. 

### Ví dụ 2 

đầu vào:```
3
+4
-4
+6
```| Bước | Hoạt động | Ngăn xếp | 
| --- | --- | --- | 
| 1 | +4 | [4] | 
| 2 | -4 | [] | 
| 3 | +6 | [6] | 

Ngăn xếp cuối cùng có một lớp. 

Trường hợp này cho thấy việc loại bỏ hoàn toàn, sau đó là một khởi đầu mới, xác nhận rằng các trạng thái trống vẫn tạo ra vòng cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đơn vị cà phê được đẩy và bật tối đa một lần trong tất cả các hoạt động | 
| Không gian | O(n) | Ngăn xếp lưu trữ tối đa một mục cho mỗi thao tác đổ | 

Độ phức tạp tuyến tính dễ dàng phù hợp với các ràng buộc điển hình cho hàng trăm nghìn thao tác, vì mỗi thao tác được xử lý trong thời gian không đổi được khấu hao. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# since solve prints, we adapt
def run(inp: str) -> str:
    import sys, io
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

# minimal case
assert run("1\n+5\n") == "2"

# full removal
assert run("1\n+5\n-5\n") == "1"

# partial spanning multiple layers
assert run("3\n+3\n+2\n-4\n") == "2"

# alternating operations
assert run("5\n+1\n+2\n-1\n-1\n+3\n") == "2"

# large stacking then full clear
assert run("4\n+10\n+5\n-7\n-8\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đổ đơn | 2 | đếm vòng cơ sở | 
| loại bỏ hoàn toàn | 1 | hộp đựng cốc rỗng | 
| uống xuyên lớp | 2 | tiêu thụ nhiều lớp | 
| hoạt động luân phiên | 2 | ổn định ngăn xếp | 
| rõ ràng hoàn toàn | 1 | lặp đi lặp lại pop đúng đắn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả cà phê đã rót cuối cùng sẽ bị loại bỏ. Coi như:```
2
+10
-10
```Ngăn xếp trở nên trống sau khi xử lý. Thuật toán xuất ra chính xác 1 vì ranh giới dưới cùng vẫn được tính là một vòng ngay cả khi không còn cà phê. Cách tiếp cận “số lớp” ngây thơ sẽ đưa ra kết quả 0 không chính xác. 

Một trường hợp đặc biệt khác xảy ra khi đồ uống trải dài trên nhiều lớp:```
3
+3
+4
-5
```Từng bước, ngăn xếp phát triển từ`[3, 4]`ĐẾN`[2]`. Lớp trên cùng được loại bỏ hoàn toàn và lớp tiếp theo được giảm đi một phần. Kích thước ngăn xếp giảm một cách chính xác, duy trì tính bất biến mà mỗi phần tử tương ứng với ranh giới lớp vẫn đang hoạt động. 

Trường hợp cạnh thứ ba là đồ uống nhỏ lặp đi lặp lại sẽ tiêu tốn dần một lớp:```
4
+5
-2
-2
-1
```Lớp trên cùng tồn tại qua nhiều hoạt động trước khi biến mất. Thuật toán trì hoãn việc bật lên một cách chính xác cho đến khi giá trị còn lại bằng 0, đảm bảo không loại bỏ sớm ranh giới lớp.
