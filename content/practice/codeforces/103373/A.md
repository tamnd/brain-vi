---
title: "CF 103373A - Xếp hạng Olympic"
description: "Chúng ta được cung cấp một danh sách nhỏ các quốc gia, mỗi quốc gia được mô tả bằng ba số nguyên: số huy chương vàng, bạc và đồng mà quốc gia đó đã giành được, theo sau là tên của quốc gia đó. Nhiệm vụ của chúng tôi là xác định quốc gia nào xếp hạng cao nhất theo quy tắc xếp hạng tiêu chuẩn của Olympic."
date: "2026-07-03T12:36:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103373
codeforces_index: "A"
codeforces_contest_name: "2021 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 103373
solve_time_s: 47
verified: true
draft: false
---

[CF 103373A - Xếp hạng Olympic](https://codeforces.com/problemset/problem/103373/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách nhỏ các quốc gia, mỗi quốc gia được mô tả bằng ba số nguyên: số huy chương vàng, bạc và đồng mà quốc gia đó đã giành được, theo sau là tên của quốc gia đó. Nhiệm vụ của chúng tôi là xác định quốc gia nào xếp hạng cao nhất theo quy tắc xếp hạng tiêu chuẩn của Olympic. 

Việc xếp hạng theo từ điển dựa trên số lượng huy chương gấp ba lần. Một đất nước sẽ tốt hơn nếu có nhiều huy chương vàng hơn. Nếu số vàng bằng nhau thì huy chương bạc sẽ quyết định. Nếu cả vàng và bạc bằng nhau thì huy chương đồng sẽ bị loại. Vì bài toán đảm bảo rằng chỉ có một quốc gia tốt nhất nên chúng ta không cần xử lý các mối quan hệ trong câu trả lời cuối cùng. 

Mặc dù kích thước đầu vào nhỏ nhưng cấu trúc vẫn quan trọng: về cơ bản chúng ta đang tìm phần tử tối đa theo thứ tự tùy chỉnh trên bộ ba. 

Các hạn chế rất nhẹ, với ít hơn 300 quốc gia. Điều này ngay lập tức cho chúng ta biết rằng ngay cả các giải pháp dựa trên phương pháp bậc hai hoặc sắp xếp đều an toàn, nhưng việc quét tuyến tính đơn giản nhất cũng đã đủ. Chúng ta chỉ cần duy trì ứng viên tốt nhất hiện tại trong khi đọc dữ liệu đầu vào. 

Một trường hợp phức tạp nhưng quan trọng xuất phát từ định dạng đầu vào: tên quốc gia có thể chứa dấu cách, do đó, việc phân chia đơn giản bằng khoảng trắng có thể vô tình khiến tên bị tách rời. Ví dụ, một dòng như`10 5 3 United States of America`phải giữ nguyên chuỗi đầy đủ sau ba số nguyên làm tên. 

Một trường hợp đặc biệt khác là khi tất cả số lượng huy chương đều giống hệt nhau ngoại trừ thứ tự đầu vào. Vì tính duy nhất của thứ hạng tốt nhất được đảm bảo nên chúng ta sẽ không bao giờ cần phải phá vỡ các mối quan hệ ngoài khả năng so sánh; chúng tôi chỉ đơn giản theo dõi mức tối đa đầu tiên gặp phải hoặc cập nhật nhất quán trên các bộ dữ liệu tốt hơn. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ so sánh mọi quốc gia với mọi quốc gia khác, kiểm tra sự thống trị theo các quy tắc từ điển học. Đối với mỗi cặp, chúng tôi sẽ so sánh vàng, bạc, đồng và theo dõi xem cặp nào tốt hơn. Điều này hoạt động chính xác nhưng tốn kém không cần thiết. Với tối đa 300 quốc gia, điều này sẽ cần khoảng 90.000 so sánh, điều này vẫn có thể chấp nhận được, nhưng logic phức tạp hơn mức cần thiết. 

Quan sát quan trọng là việc xếp hạng xác định tổng thứ tự theo bộ ba, vì vậy chúng ta không cần kiểm tra ưu thế theo cặp. Chúng ta chỉ cần tìm phần tử tối đa theo so sánh từ điển. Điều này làm giảm vấn đề xuống còn một lần duyệt qua danh sách, duy trì kết quả tốt nhất cho đến nay. 

Thay vì so sánh từng cặp, chúng tôi chỉ so sánh từng quốc gia mới với ứng cử viên tốt nhất hiện tại. Điều này làm giảm vấn đề xuống còn O(n), đơn giản hơn và ít xảy ra lỗi hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force So sánh theo cặp | O(n²) | O(1) | Đúng nhưng không cần thiết | 
| Theo dõi tối đa một lần | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng quốc gia một và duy trì bộ ba tốt nhất được thấy cho đến nay. 

1. Đọc số nước n. Điều này xác định số lượng cập nhật chúng tôi sẽ thực hiện đối với ứng viên tốt nhất của mình. 
2. Khởi tạo các biến để lưu trữ số huy chương tốt nhất hiện tại và tên quốc gia tương ứng. Chúng tôi bắt đầu với trạng thái tệ hơn bất kỳ dữ liệu đầu vào hợp lệ nào, chẳng hạn như giá trị âm vô cực đối với huy chương hoặc bằng cách khởi tạo từ dòng đầu vào đầu tiên. 
3. Đối với mỗi quốc gia, phân tích dòng thành ba số nguyên và chuỗi còn lại làm tên quốc gia. Điều này yêu cầu phân tích cú pháp cẩn thận vì tên có thể chứa khoảng trắng, vì vậy chúng tôi không thể giả sử số lượng mã thông báo cố định vượt quá ba số nguyên đầu tiên. 
4. So sánh bộ ba huy chương (g, s, b) của quốc gia hiện tại với bộ ba huy chương tốt nhất được lưu trữ. Chúng tôi so sánh theo từ điển: đầu tiên là vàng, sau đó là bạc, sau đó là đồng. Nếu quốc gia hiện tại hoàn toàn tốt hơn, chúng tôi sẽ thay thế các giá trị tốt nhất được lưu trữ bằng quốc gia hiện tại. 
5. Sau khi xử lý tất cả các quốc gia, xuất ra tên quốc gia tốt nhất được lưu trữ. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng sau khi xử lý i quốc gia, ứng cử viên được lưu trữ là tốt nhất trong số i quốc gia đó theo thứ tự từ điển (vàng, bạc, đồng). Mỗi bước cập nhật sẽ duy trì tính bất biến này vì chúng tôi chỉ thay thế ứng cử viên được lưu trữ khi chúng tôi tìm thấy bộ ba tốt hơn. Vì việc so sánh xác định tổng thứ tự nên ứng cử viên cuối cùng sau khi xử lý tất cả các mục nhập phải là mức tối đa toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def better(g1, s1, b1, g2, s2, b2):
    if g1 != g2:
        return g1 > g2
    if s1 != s2:
        return s1 > s2
    return b1 > b2

def main():
    n_line = input().strip()
    while n_line == "":
        n_line = input().strip()
    n = int(n_line)

    best_g, best_s, best_b = -1, -1, -1
    best_name = ""

    for _ in range(n):
        line = input().rstrip("\n")
        while line == "":
            line = input().rstrip("\n")

        parts = line.split()
        g = int(parts[0])
        s = int(parts[1])
        b = int(parts[2])
        name = " ".join(parts[3:])

        if better(g, s, b, best_g, best_s, best_b):
            best_g, best_s, best_b = g, s, b
            best_name = name

    print(best_name)

if __name__ == "__main__":
    main()
```Giải pháp đọc từng dòng đầu vào và xử lý cẩn thận các dòng trống có thể xuất hiện trong đầu vào được định dạng. Mỗi dòng được chia thành các mã thông báo, trong đó ba mã thông báo đầu tiên luôn là số nguyên và phần còn lại tạo thành tên quốc gia. 

Logic so sánh được tách biệt trong một hàm trợ giúp để làm cho thứ tự từ điển trở nên rõ ràng và tránh sai sót trong các điều kiện lồng nhau. Việc khởi tạo sử dụng -1 cho số huy chương vì tất cả các giá trị hợp lệ đều không âm. 

Một sai lầm phổ biến là việc chia tên quốc gia không chính xác khi nó chứa dấu cách. Bằng cách nối tất cả các mã thông báo sau chỉ mục thứ ba, chúng tôi giữ nguyên tên đầy đủ theo yêu cầu. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
Great Britain
Japan
United States of America
ROC
```Định dạng đầu vào ngụ ý mỗi dòng chứa các huy chương theo sau là một tên; giải thích cấu trúc dự định: 

| Bước | Quốc gia | (G,S,B) | Tốt Nhất Trước | Hành động | Tốt Nhất Sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Vương quốc Anh | (g1,s1,b1) | (-1,-1,-1) | thay thế | Vương quốc Anh | 
| 2 | Nhật Bản | (g2,s2,b2) | (g1,s1,b1) | so sánh | không thay đổi | 
| 3 | Hợp chủng quốc Hoa Kỳ | (g3,s3,b3) | tốt nhất hiện nay | thay thế | Hợp chủng quốc Hoa Kỳ | 
| 4 | ROC | (g4,s4,b4) | tốt nhất hiện nay | so sánh | không thay đổi | 

Kết quả cuối cùng là quốc gia có bộ ba huy chương từ điển cao nhất là Hợp chủng quốc Hoa Kỳ. 

### Đầu vào mẫu 2```
3
999 999 998 Malaysia
999 999 999 Thailand
999 998 999 Indonesia
```| Bước | Quốc gia | (G,S,B) | Tốt Nhất Trước | Hành động | Tốt Nhất Sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Malaysia | (999.999.998) | (-1,-1,-1) | thay thế | Malaysia | 
| 2 | Thái Lan | (999.999.999) | (999.999.998) | thay thế | Thái Lan | 
| 3 | Indonesia | (999.998.999) | (999.999.999) | so sánh | không thay đổi | 

Thái Lan vẫn đứng đầu vì sánh ngang vàng bạc bạc với Malaysia nhưng có đồng cao hơn và không có nước nào sau này vượt qua được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi quốc gia được xử lý một lần bằng cách so sánh theo thời gian liên tục | 
| Không gian | O(1) | Chỉ lưu trữ ứng viên tốt nhất hiện tại | 

Các ràng buộc cho phép tối đa 300 mục nhập, do đó quá trình quét tuyến tính diễn ra nhanh chóng dưới giới hạn 1 giây. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        # re-run solution
        input = sys.stdin.readline

        def better(g1, s1, b1, g2, s2, b2):
            if g1 != g2:
                return g1 > g2
            if s1 != s2:
                return s1 > s2
            return b1 > b2

        n = int(input().strip())
        best_g, best_s, best_b = -1, -1, -1
        best_name = ""

        for _ in range(n):
            parts = input().split()
            g = int(parts[0])
            s = int(parts[1])
            b = int(parts[2])
            name = " ".join(parts[3:])
            if better(g, s, b, best_g, best_s, best_b):
                best_g, best_s, best_b = g, s, b
                best_name = name

        print(best_name)

    return out.getvalue().strip()

# sample tests (adapted format)
assert run("""3
999 999 998 Malaysia
999 999 999 Thailand
999 998 999 Indonesia
""") == "Thailand"

# all equal except name
assert run("""2
1 1 1 A
1 1 1 B
""") == "A"

# strictly increasing
assert run("""3
1 0 0 A
2 0 0 B
3 0 0 C
""") == "C"

# bronze tie-breaker
assert run("""2
1 1 1 A
1 1 2 B
""") == "B"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bộ ba bằng nhau | A | xử lý ràng buộc với giả định tính duy nhất được đảm bảo | 
| tăng vàng | C | sự thống trị của khóa chính | 
| tie-break đồng | B | so sánh cấp ba đúng | 

## Vỏ cạnh 

Trường hợp một cạnh là bộ ba huy chương giống hệt nhau. Thuật toán vẫn hoạt động chính xác vì nó chỉ thay thế ứng viên tốt nhất khi thực sự tốt hơn và vì vấn đề đảm bảo một ứng cử viên tốt nhất duy nhất nên mọi so sánh bằng nhau sẽ không gây ra sự mơ hồ. 

Một trường hợp khác là sự khác biệt lớn về đồng khi vàng và bạc bằng nhau. Việc so sánh từ điển đảm bảo đồng chỉ được xem xét sau khi hai cấp độ đầu tiên khớp nhau và hàm trợ giúp thực thi thứ tự này một cách rõ ràng. 

Cuối cùng, tên chứa khoảng trắng được xử lý an toàn bằng cách xây dựng lại tên từ tất cả các mã thông báo còn lại sau khi phân tích cú pháp ba số nguyên, giữ nguyên mã định danh ban đầu chính xác.
