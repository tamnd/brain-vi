---
title: "CF 104915B - \u0410\u0431\u0441\u043e\u043b\u044e\u0442\u043d\u043e \u043e\u0442\u043d\u043e\u0441\u0438\u0442\u0435\u043b\u044c\u043d\u043e"
description: "Chúng ta được cung cấp một tuyến đường được viết theo hướng la bàn tuyệt đối, trong đó mỗi bước là một trong bốn giá trị: Bắc, Đông, Nam hoặc Tây. Nền tảng thực hiện tuyến đường bắt đầu với hướng cố định, ban đầu hướng về phía Bắc."
date: "2026-06-28T18:05:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104915
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u0421\u0430\u043c\u0430\u0440\u0435 2023-2024 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104915
solve_time_s: 46
verified: true
draft: false
---

[CF 104915B - \u0410\u0431\u0441\u043e\u043b\u044e\u0442\u043d\u043e \u043e\u0442\u043d\u043e\u0441\u0438\u0442\u0435\u043b\u044c\u043d\u043e](https://codeforces.com/problemset/problem/104915/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tuyến đường được viết theo hướng la bàn tuyệt đối, trong đó mỗi bước là một trong bốn giá trị: Bắc, Đông, Nam hoặc Tây. Nền tảng thực hiện tuyến đường bắt đầu với hướng cố định, ban đầu hướng về phía Bắc. Khi lộ trình được thực thi, mỗi lệnh mới được đưa ra trong hệ tọa độ chung, nhưng điều chúng ta muốn là viết lại chuyển động tương tự như một chuỗi các lệnh tương đối từ góc độ của nền tảng chuyển động. 

Sự chuyển đổi quan trọng là ở mỗi bước, thay vì diễn giải hướng dưới dạng vectơ tuyệt đối, chúng tôi diễn giải hướng đó theo hướng hiện tại của nền tảng. Sau mỗi lần di chuyển, hướng của nền tảng được cập nhật để khớp với hướng mà nó vừa di chuyển và lệnh tiếp theo phải được thể hiện liên quan đến hướng được cập nhật này. 

Do đó, đầu ra là một chuỗi có cùng độ dài, nhưng thay vì hướng tuyệt đối, nó mã hóa xem nền tảng tiếp tục tiến về phía trước, rẽ phải, quay lại hay rẽ trái so với hướng hiện tại của nó. 

Các ràng buộc đủ nhỏ để chỉ cần quét tuyến tính trên chuỗi là đủ. Bất kỳ giải pháp nào cố gắng tính toán lại các hướng tương đối từ đầu cho từng vị trí sẽ vẫn là tuyến tính, nhưng bất kỳ giải pháp nào liên quan đến tính toán lại theo cặp hoặc tính toán trước tất cả các chuyển đổi đều là chi phí không cần thiết. Độ phức tạp của mục tiêu tự nhiên là O(n). 

Một trường hợp phức tạp xuất phát từ việc xử lý tính bao bọc trong số học định hướng. Vì các hướng tạo thành một chu trình có kích thước bốn, phép trừ ngây thơ có thể tạo ra các giá trị âm. Ví dụ: di chuyển từ Bắc sang Tây sẽ tạo ra chênh lệch thô là -1, chênh lệch này phải được chuẩn hóa thành phạm vi hợp lệ [0, 3]. Bất kỳ triển khai nào quên chuẩn hóa mô-đun sẽ âm thầm tạo ra các lệnh tương đối không chính xác trong các chuyển đổi bao quanh đó. 

Một cạm bẫy phổ biến khác là hiểu lầm rằng hướng của nền tảng sẽ thay đổi sau mỗi bước. Ví dụ: nếu chúng ta giả định không chính xác hướng tham chiếu cố định (luôn là hướng Bắc), chúng ta sẽ tính toán các hướng tương đối luôn sai sau lần di chuyển đầu tiên. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ mô phỏng nền tảng từng bước và ở mỗi bước sẽ tính toán lại hướng tương đối bằng cách so sánh hướng tuyệt đối của lệnh hiện tại với hướng hiện tại. Điều này đã gần đạt đến mức tối ưu, nhưng việc triển khai bất cẩn có thể tính toán lại những khác biệt bằng cách quét hoặc ánh xạ qua các cặp nhiều lần, dẫn đến công việc dư thừa. 

Quan sát rõ ràng là cả hướng tuyệt đối và chuyển tiếp tương đối đều tồn tại trong cùng một nhóm tuần hoàn có kích thước bốn. Nếu chúng ta mã hóa các hướng dưới dạng số nguyên Bắc, Đông, Nam, Tây được ánh xạ thành 0, 1, 2, 3 thì lệnh tương đối chỉ đơn giản là sự khác biệt mô-đun giữa hướng mục tiêu và hướng hiện tại. Sau khi thực hiện một bước, chúng tôi cập nhật hướng hiện tại theo cùng hướng đích đó. 

Điều này làm giảm vấn đề trong việc duy trì một trạng thái số nguyên duy nhất và thực hiện số học theo thời gian không đổi cho mỗi ký tự. Ý tưởng brute-force hoạt động được vì nó đã tuân theo cấu trúc mô phỏng, nhưng nó trở nên phức tạp không cần thiết nếu chúng ta nghĩ về các vectơ hình học hoặc các phép biến đổi dựa trên chuỗi. Một khi chúng ta nhận ra cấu trúc tuần hoàn, mọi thứ sẽ sụp đổ thành số học mô-đun đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force với tính toán lại | O(n) | O(1) | Được chấp nhận nhưng dài dòng | 
| Mã hóa mô-đun tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi từng ký tự hướng thành một số nguyên trong một chu trình: Bắc trở thành 0, Đông 1, Nam 2, Tây 3. Điều này cho phép các phép tính số học biểu diễn các phép quay. 
2. Khởi tạo hướng hiện tại là 0, vì nền tảng bắt đầu hướng về phía Bắc. 
3. Đối với mỗi lệnh trong chuỗi đầu vào, hãy tính toán sự khác biệt giữa hướng lệnh và hướng hiện tại bằng phép trừ. 
4. Bình thường hóa sự khác biệt này bằng cách sử dụng modulo 4. Nếu kết quả là âm, hãy ngầm cộng 4 thông qua số học modulo. Điều này đảm bảo chúng ta luôn ở trong các trạng thái định hướng hợp lệ. 
5. Chuyển giá trị kết quả trở lại thành lệnh tương đối: 0 nghĩa là tiếp tục tiến lên, 1 nghĩa là rẽ phải, 2 nghĩa là quay lại, 3 nghĩa là rẽ trái. 
6. Cập nhật hướng hiện tại thành hướng tuyệt đối vừa thực hiện, vì nền tảng quay về hướng chuyển động của nó. 
7. Nối lệnh tương đối vào chuỗi đầu ra và tiến hành ký tự đầu vào tiếp theo. 

Ý tưởng cốt lõi là ở mỗi bước, chúng ta thể hiện hướng tuyệt đối dưới dạng một phép quay so với tiêu đề hiện tại, sau đó cập nhật tiêu đề theo hướng đó. Sự phát triển trạng thái được nắm bắt hoàn toàn bởi một số nguyên duy nhất. 

### Tại sao nó hoạt động 

Thuật toán duy trì bất biến rằng hướng được lưu trữ luôn bằng hướng tuyệt đối được thực hiện cuối cùng. Bởi vì mọi lệnh tương đối được tính là sự khác biệt giữa hướng tuyệt đối tiếp theo và hướng được lưu trữ này trong chu kỳ modulo-4, nên nó mã hóa chính xác phép quay cần thiết để chuyển đổi tiêu đề này sang tiêu đề khác. Do các phép quay theo bốn hướng tạo thành một nhóm tuần hoàn khép kín theo phép cộng modulo 4, nên mỗi phép biến đổi được biểu diễn duy nhất và không có sự mơ hồ hoặc độ trôi tích lũy qua các bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    mp = {
        'N': 0,
        'E': 1,
        'S': 2,
        'W': 3
    }

    rmp = {
        0: 'N',
        1: 'E',
        2: 'S',
        3: 'W'
    }

    cur = 0
    out = []

    for ch in s:
        nxt = mp[ch]
        diff = (nxt - cur) % 4
        out.append(rmp[diff])
        cur = nxt

    sys.stdout.write(''.join(out))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc duy trì một biến duy nhất`cur`, đại diện cho định hướng tuyệt đối hiện tại của nền tảng. Đối với mỗi ký tự, chúng tôi chuyển đổi nó thành mã hóa số và tính toán sự khác biệt theo mô-đun. Hoạt động modulo xử lý thống nhất cả trường hợp bao bọc xuôi và trừ âm, do đó không cần logic điều kiện bổ sung. 

bản cập nhật`cur = nxt`là cần thiết vì nó phản ánh chuyển động quay vật lý của nền tảng sau khi thực hiện mỗi lần di chuyển. Nếu không có bản cập nhật này, tất cả các tính toán tương đối tiếp theo sẽ có hướng cố định không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
NWES
```Chúng tôi theo dõi hướng hiện tại và tính toán các bước di chuyển tương đối. 

| Bước | Đầu vào | Hiện tại | Tiếp theo | Sự khác biệt | Đầu ra | Hiện Tại Mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | N | 0 | 0 | 0 | N | 0 | 
| 2 | W | 0 | 3 | 3 | W | 3 | 
| 3 | E | 3 | 1 | 2 | S | 1 | 
| 4 | S | 1 | 2 | 1 | E | 2 | 

Đầu ra:```
NWSE
```Dấu vết này cho thấy hướng liên tục thay đổi như thế nào, gây ra các chuyển động tuyệt đối giống hệt nhau để ánh xạ tới các lệnh tương đối khác nhau tùy thuộc vào trạng thái. 

### Ví dụ 2 

đầu vào:```
NESW
```| Bước | Đầu vào | Hiện tại | Tiếp theo | Sự khác biệt | Đầu ra | Hiện Tại Mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | N | 0 | 0 | 0 | N | 0 | 
| 2 | E | 0 | 1 | 1 | E | 1 | 
| 3 | S | 1 | 2 | 1 | E | 2 | 
| 4 | W | 2 | 3 | 1 | E | 3 | 

Đầu ra:```
NEEE
```Ví dụ này nêu bật cách lặp lại chuyển động tương đối thuận vẫn có thể tương ứng với việc thay đổi hướng tuyệt đối do khung quay. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với các phép toán số học O(1) | 
| Không gian | O(1) | Chỉ sử dụng bộ đệm đầu ra và ánh xạ có kích thước không đổi | 

Thuật toán khớp trực tiếp với kích thước đầu vào, do đó, ngay cả đầu vào lớn nhất có thể cũng được xử lý thoải mái trong giới hạn thời gian. Không cần đệ quy, không có vòng lặp lồng nhau và không cần cấu trúc dữ liệu phụ trợ có thể mở rộng theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# simple cases
assert run("N") == "N"
assert run("NE") == "NE"

# sample-like case
assert run("NWES") == "NWSE"

# alternating turns
assert run("NESW") == "NEEE"

# all same direction
assert run("NNNN") == "NNNN"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N | N | trường hợp nhận dạng một bước | 
| Tây Bắc | Tây Bắc | luân chuyển hỗn hợp và cập nhật trạng thái | 
| NNNN | NNNN | không tích lũy luân chuyển | 
| TIN TỨC | NEEE | xoay khung nhất quán | 

## Vỏ cạnh 

Trường hợp một cạnh là khi hướng nhảy lùi trong chu kỳ, chẳng hạn như chuyển từ Bắc sang Tây. Đối với đầu vào`NW`, phép tính đi từ cur = 0 đến nxt = 3, tạo ra (3 - 0) % 4 = 3, ánh xạ chính xác tới việc rẽ trái. Phép trừ ngây thơ không xử lý modulo sẽ mang lại -1 và không ánh xạ tới lệnh hợp lệ. 

Một trường hợp khác là các phép quay lặp đi lặp lại sẽ tích lũy các thay đổi về hướng. Đối với đầu vào`NESW`, định hướng hiện tại phát triển qua cả bốn trạng thái. Thuật toán cập nhật chính xác`cur`ở mỗi bước, do đó, mỗi sự khác biệt sẽ được tính toán tương ứng với khung hình chính xác, ngăn ngừa hiện tượng trôi. 

Trường hợp tinh tế cuối cùng là các chuỗi dài đồng nhất như`SSSSSS`. Vì mỗi bước cập nhật hướng về phía Nam nhiều lần nên mọi khác biệt đều bằng 0 sau lần di chuyển đầu tiên, tạo ra các lệnh chuyển tiếp nhất quán. Điều này xác nhận rằng cập nhật trạng thái là bình thường khi chỉ đường lặp lại.
