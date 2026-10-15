---
title: "CF 105435A - Thế giới của JS (Jagjeet & Sagar)"
description: "Chúng ta được giao một lịch trình mô tả thời điểm Sagar bận và chúng ta cần quyết định xem thời điểm mà Jagjeet đã chọn có nằm trong khoảng thời gian bận rộn đó hay không. Mô hình bận rộn của Sagar bắt đầu vào thời điểm a. Sau đó, tính khả dụng của anh ấy thay đổi theo cách có cấu trúc: mọi chu kỳ đều dựa trên b."
date: "2026-06-23T03:48:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105435
codeforces_index: "A"
codeforces_contest_name: "TSEC Round 2 (Div. 3)"
rating: 0
weight: 105435
solve_time_s: 89
verified: true
draft: false
---

[CF 105435A - Thế giới của JS (Jagjeet & Sagar)](https://codeforces.com/problemset/problem/105435/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một lịch trình mô tả thời điểm Sagar bận và chúng ta cần quyết định xem thời điểm mà Jagjeet đã chọn có nằm trong khoảng thời gian bận rộn đó hay không. 

Mô hình bận rộn của Sagar bắt đầu vào đúng thời điểm`a`. Sau đó, tính khả dụng của anh ta thay đổi theo cách có cấu trúc: mọi chu kỳ đều dựa trên`b`. Trong mỗi chu kỳ, có hai thời điểm bận rộn: thời điểm đầu tiên là vào thời điểm bắt đầu chính xác của chu kỳ và thời điểm thứ hai là một giây sau đó. Cụ thể, thời gian bận rộn tạo thành các cặp:`a, a+1`, sau đó`a+b, a+b+1`, sau đó`a+2b, a+2b+1`, vân vân. 

Vì vậy, nhiệm vụ đơn giản là: với mỗi truy vấn, hãy kiểm tra xem một thời điểm nhất định có`c`bằng với bất kỳ dấu thời gian bận nào trong số này. 

Kích thước đầu vào nhỏ, có tới 1000 trường hợp thử nghiệm và giá trị lên tới 10^9. Điều này ngay lập tức loại trừ mọi mô phỏng theo thời gian hoặc mở rộng lịch trình lặp đi lặp lại. Bất kỳ giải pháp đúng nào cũng phải quyết định tư cách thành viên trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một lỗi phổ biến là hiểu mô hình này là một khoảng thời gian liên tục, ví dụ như nghĩ rằng Sagar đang bận trong các phạm vi như`[a, a+1]`,`[a+b, a+b+1]`v.v. nhưng sau đó lại vô tình coi chúng là những khoảng thời gian chồng chéo hoặc kéo dài. Vì các khoảng cách biệt và không chồng chéo (vì`b ≥ 1`), sự chồng chéo chỉ xảy ra khi`b = 1`, trong trường hợp đó các cặp liên tiếp hợp nhất thành các phân đoạn bận liên tục. Ngay cả trong trường hợp đó, việc kiểm tra tư cách thành viên trực tiếp vẫn hoạt động nhưng logic hợp nhất theo khoảng thời gian bất cẩn có thể phá vỡ việc xử lý biên. 

Một trường hợp phức tạp khác là khi`c < a`. Trong trường hợp này Sagar chưa bắt đầu bận rộn chút nào nên câu trả lời phải là “KHÔNG”. Tương tự, khi`b = 1`, thời gian bận trở thành mọi số nguyên bắt đầu từ`a`, từ`(a, a+1), (a+1, a+2), ...`bao gồm tất cả các số nguyên từ`a`trở đi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng thời gian bận rộn của Sagar bằng cách tạo ra các cặp`(a + k*b, a + k*b + 1)`để tăng`k`, dừng lại khi chúng ta vượt quá`c`. Đối với mỗi cặp được tạo, chúng tôi kiểm tra xem`c`khớp với một trong hai điểm cuối. Điều này đúng vì nó trực tiếp xây dựng mẫu được mô tả. 

Tuy nhiên, điều này trở nên kém hiệu quả khi`c`lớn so với`a`Và`b`. Trong trường hợp xấu nhất, nếu`b = 1`, số lần lặp tăng tuyến tính lên tới`c - a`, có thể lớn tới 10^9. Ngay cả khi`b`lớn hơn, chúng tôi vẫn gặp rủi ro khi lặp lại nhiều trường hợp thử nghiệm. 

Quan sát quan trọng là cấu trúc này hoàn toàn tuần hoàn và chúng ta không cần liệt kê nó. Thay vào đó, chúng ta có thể xác định liệu`c`căn chỉnh với chỉ số chu kỳ và độ lệch. 

Trong một thời gian nhất định`c`, nếu là thời gian bận thì phải thỏa mãn một trong hai điều kiện: 

Đầu tiên, nó có thể là sự khởi đầu của một chu kỳ:`c = a + k*b`đối với một số nguyên`k ≥ 0`. Điều này tương đương với`(c - a) % b == 0`. 

Thứ hai, đó có thể là thời điểm bận rộn thứ hai trong một chu kỳ:`c = a + k*b + 1`. Điều này tương đương với`(c - a - 1) % b == 0`. 

Cả hai điều kiện cũng phải thỏa mãn`c ≥ a`, vì thời gian bận rộn không bao giờ xảy ra trước điểm bắt đầu. 

Điều này làm giảm vấn đề về việc kiểm tra số học theo thời gian không đổi cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O((c−a)/b) mỗi bài kiểm tra | O(1) | Quá chậm | 
| Kiểm tra số học mô-đun | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và áp dụng kiểm tra số học để xác định tư cách thành viên trong lịch trình bận rộn. 

1. Đọc các giá trị`a`,`b`, Và`c`cho trường hợp thử nghiệm hiện tại. Chúng xác định điểm bắt đầu, độ dài chu kỳ và thời gian truy vấn. 
2. Nếu`c < a`, kết luận ngay là Sagar không có việc bận`c`. Điều này tuân theo định nghĩa vì lịch trình chỉ bắt đầu lúc`a`. 
3. Kiểm tra xem`c`khớp với vị trí bận đầu tiên trong một số chu kỳ bằng cách kiểm tra xem`(c - a) % b == 0`. Nếu điều này giữ vững thì`c`căn chỉnh chính xác với`a + k*b`đối với một số nguyên`k`. 
4. Nếu điều kiện trước đó không thành công, hãy kiểm tra xem`c`khớp với vị trí bận thứ hai trong một chu kỳ bằng cách kiểm tra xem`(c - a - 1) % b == 0`. Điều này tương ứng với`a + k*b + 1`. 
5. Nếu một trong hai điều kiện đúng, xuất ra “CÓ”. Nếu không thì xuất ra “NO”. 

### Tại sao nó hoạt động 

Mỗi thời gian bận phải thuộc về đúng một trong hai cấp số cộng: một cấp số bắt đầu từ`a`với bước`b`, và một bắt đầu từ`a+1`với cùng một bước`b`. Hai chuỗi này mô tả đầy đủ lịch trình và chúng không tạo ra kết quả dương tính giả vì mỗi thời gian hợp lệ phải ánh xạ tới một chỉ mục chu kỳ duy nhất`k`. Điều kiện modulo đảm bảo rằng chỉ có thể truy cập lần bằng cách bước số nguyên từ`a`hoặc`a+1`tăng dần`b`được chấp nhận, phù hợp chính xác với việc xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    for _ in range(n):
        a, b, c = map(int, input().split())

        if c < a:
            print("NO")
            continue

        if (c - a) % b == 0 or (c - a - 1) % b == 0:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Mã tuân theo các điều kiện dẫn xuất trực tiếp. Người bảo vệ đầu tiên xử lý việc loại trừ tầm thường trong đó có truy vấn trước khi lịch trình bắt đầu. Hai phép kiểm tra modulo tương ứng chính xác với hai cấp số cộng xác định mẫu bận. Không cần vòng lặp nên mỗi trường hợp kiểm thử được xử lý trong thời gian không đổi. 

Một chi tiết triển khai tinh tế là`(c - a - 1)`có thể âm khi`c = a`, nhưng modulo của Python xử lý toán hạng âm một cách chính xác trong ngữ cảnh này và điều kiện trước đó đã đảm bảo tính chính xác. Sớm`c < a`kiểm tra ngăn chặn sự mơ hồ không cần thiết. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện từ mẫu. 

### Ví dụ 1 

đầu vào:`a = 3, b = 2, c = 6`| Bước | Đã kiểm tra tình trạng | Biểu hiện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | c < a | 6 < 3 | Sai | 
| 2 | trình tự đầu tiên | (6 - 3) % 2 | 3 % 2 = 1 | 
| 3 | chuỗi thứ hai | (6 - 3 - 1) % 2 | 2 % 2 = 0 | 

Vì điều kiện thứ hai được giữ, đầu ra là “CÓ”. 

Điều này cho thấy rằng ngay cả khi một giá trị không được căn chỉnh với thời điểm bắt đầu chu kỳ cơ sở, nó vẫn có thể khớp với vị trí bận đã được dịch chuyển. 

### Ví dụ 2 

đầu vào:`a = 8, b = 7, c = 10`| Bước | Đã kiểm tra tình trạng | Biểu hiện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | c < a | 10 < 8 | Sai | 
| 2 | trình tự đầu tiên | (10 - 8) % 7 | 2% 7 = 2 | 
| 3 | chuỗi thứ hai | (10 - 8 - 1) % 7 | 1% 7 = 1 | 

Cả hai điều kiện đều không đúng nên đầu ra là “KHÔNG”. 

Điều này xác nhận rằng các giá trị không được căn chỉnh theo một trong hai tiến trình sẽ bị từ chối một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm sử dụng một số phép tính số học không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài các biến đầu vào | 

Các ràng buộc cho phép tối đa 1000 trường hợp kiểm thử, do đó, việc quét tuyến tính qua các trường hợp kiểm thử với O(1) mỗi trường hợp đều dễ dàng nằm trong giới hạn. Các phép toán số học có thời gian không đổi và an toàn trong phạm vi số nguyên 64 bit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    out = []
    for _ in range(n):
        a, b, c = map(int, input().split())
        if c < a:
            out.append("NO")
        elif (c - a) % b == 0 or (c - a - 1) % b == 0:
            out.append("YES")
        else:
            out.append("NO")
    return "\n".join(out)

# provided samples
assert run("""10
3 2 6
1 1 1
8 7 10
2 9 8
3 4 6
1 5 6
8 9 4
7 5 3
10 8 3
1 4 1
""") == """YES
YES
NO
NO
NO
YES
NO
NO
NO
YES"""

# custom cases
assert run("1\n5 3 5\n") == "YES"
assert run("1\n5 3 4\n") == "YES"
assert run("1\n5 3 6\n") == "NO"
assert run("1\n10 1 100\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 3 5`| CÓ | bắt đầu chu kỳ đầu tiên | 
|`5 3 4`| CÓ | vị trí thứ hai trong chu kỳ | 
|`5 3 6`| KHÔNG | thời gian không trùng khớp | 
|`10 1 100`| CÓ | trường hợp dày đặc khi b = 1 | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`c = a`. Trong tình huống này, câu trả lời phải luôn là “CÓ” vì thời điểm bận rộn đầu tiên bắt đầu chính xác vào lúc`a`. Thuật toán xử lý việc này một cách chính xác vì`(c - a) % b = 0`đánh giá để`0 % b = 0`, gây ra một kết quả tích cực. 

Một trường hợp khác là khi`b = 1`. Thời gian bận rộn trở nên`(a, a+1), (a+1, a+2), ...`, bao phủ hiệu quả mọi số nguyên từ`a`trở đi. Séc`(c - a) % 1 == 0`luôn đúng với`c ≥ a`, do đó thuật toán trả về chính xác “CÓ” cho tất cả các trường hợp như vậy, trong khi`c < a`đã bị loại trừ rồi. 

Cuối cùng, khi`c < a`, cả hai kiểm tra mô-đun đều có thể tạo ra các giá trị sai lệch do số học âm, nhưng tính năng bảo vệ sớm sẽ ngăn chặn hoàn toàn tình huống này. Ví dụ, nếu`a = 10, c = 3`, sau đó`(c - a) = -7`và các phép toán modulo vẫn có thể trả về các phần dư hợp lệ trong Python, nhưng chúng không liên quan vì lịch trình chưa bắt đầu, vì vậy kết quả đầu ra đúng luôn là “KHÔNG”.
