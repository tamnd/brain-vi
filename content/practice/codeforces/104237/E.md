---
title: "CF 104237E - Đếm cây"
description: "Chúng ta được cho một dãy các khối, mỗi khối có một số cây nhất định. Một loạt các bước đi cũng được cung cấp, trong đó mỗi bước đi bao gồm một đoạn khối liền kề từ điểm cuối này đến điểm cuối khác."
date: "2026-07-01T23:20:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104237
codeforces_index: "E"
codeforces_contest_name: "Harker Programming Invitational 2023 Novice"
rating: 0
weight: 104237
solve_time_s: 70
verified: true
draft: false
---

[CF 104237E - Đếm cây](https://codeforces.com/problemset/problem/104237/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các khối, mỗi khối có một số cây nhất định. Một loạt các bước đi cũng được cung cấp, trong đó mỗi bước đi bao gồm một đoạn khối liền kề từ điểm cuối này đến điểm cuối khác. Với mỗi lần đi bộ, Clare đi qua tất cả các cây nằm trên mỗi khối trong khoảng thời gian đó và nhiệm vụ là tính tổng số cây cô ấy đi qua tất cả các lần đi bộ cộng lại. 

Việc đọc trực tiếp bài toán cho thấy chúng ta cần phải tính tổng các giá trị nhiều lần trên nhiều mảng con của một mảng cố định. Độ dài mảng tối đa là 200, trong khi số lượng truy vấn có thể lên tới 10.000. Mỗi giá trị đều nhỏ nhưng sự tích lũy trên nhiều phạm vi mới là điều quan trọng. 

Các ràng buộc ngay lập tức gợi ý rằng việc tính toán lại tổng từng phân đoạn từ đầu là khó khăn nhưng có thể chấp nhận được. Tuy nhiên, thực hiện 10.000 truy vấn, mỗi lần quét tối đa 200 phần tử sẽ dẫn đến khoảng 2 triệu thao tác, gần bằng giới hạn trên của mức có thể chấp nhận được trong Python dưới giới hạn 1 giây. Bất kỳ chi phí bổ sung nào hoặc nhiều trường hợp thử nghiệm sẽ gây ra rủi ro. Một giải pháp sạch hơn nên tránh việc truyền tải lặp đi lặp lại. 

Một điểm tinh tế là mỗi điểm cuối của bước đi có thể được đưa ra theo bất kỳ thứ tự nào. Việc đi bộ từ khối 5 đến khối 2 cũng giống như từ khối 2 đến khối 5. Bất kỳ giải pháp nào giả định A_i luôn nhỏ hơn hoặc bằng B_i sẽ thất bại. 

Một sai lầm phổ biến khác là quên mất tính toàn diện. Vì cả hai điểm cuối đều được bao gồm nên phân đoạn như 1 đến 1 vẫn đóng góp giá trị của khối đơn lẻ đó chứ không phải bằng 0. 

Các trường hợp cạnh xuất hiện khi phạm vi đi bộ thu gọn thành một khối duy nhất hoặc khi tất cả các cây tập trung ở một vị trí. Ví dụ: nếu chúng ta có N = 1, T = [7] và một truy vấn duy nhất (1, 1), câu trả lời đúng là 7. Việc triển khai bất cẩn khi sử dụng các khoảng nửa mở hoặc cắt lát không chính xác sẽ tạo ra 0. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng bước đi một cách độc lập bằng cách lặp qua tất cả các khối trong phạm vi của nó và tính tổng các cây. Điều này rất đơn giản: đối với mỗi truy vấn, chúng tôi chuẩn hóa các điểm cuối để luôn lặp từ chỉ mục nhỏ hơn đến chỉ mục lớn hơn, sau đó tích lũy các giá trị trong khoảng đó. 

Cách tiếp cận này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề mà không cần chuyển đổi. Tuy nhiên, chi phí của nó tỷ lệ tuyến tính với kích thước của mỗi khoảng truy vấn. Trong trường hợp xấu nhất, mỗi truy vấn bao trùm gần như toàn bộ mảng, dẫn đến các phép toán khoảng M × N, tức là khoảng 2 × 10^6 phép toán trong trường hợp xấu nhất ở đây. Điều đó vẫn khó có thể thực hiện được, nhưng nó chặt chẽ một cách không cần thiết và phải làm đi làm lại nhiều lần. 

Quan sát chính là mảng cơ bản là tĩnh. Mọi truy vấn đều yêu cầu tính tổng trên một phạm vi của một mảng cố định và có rất nhiều truy vấn như vậy. Cấu trúc này chính xác là mục đích của tổng tiền tố. Bằng cách tính trước các tổng tích lũy một lần, mỗi truy vấn có thể được trả lời trong thời gian không đổi bằng cách trừ hai giá trị tiền tố. Toàn bộ chi phí của việc tính tổng phạm vi lặp đi lặp lại được gộp lại thành một bước tiền xử lý duy nhất. 

Phương pháp brute-force hoạt động vì dữ liệu nhỏ nhưng lại thất bại về mặt hiệu quả và khả năng mở rộng. Phép biến đổi tổng tiền tố thay thế việc truyền tải lặp lại bằng việc sử dụng lại các kết quả từng phần đã được tính toán trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(MN) | O(1) | Quá chậm dưới những hạn chế chặt chẽ | 
| Tiền tố Tổng | O(N + M) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mảng thành cấu trúc cho phép truy vấn trong phạm vi thời gian không đổi.

1. Đọc mảng cây và xây dựng một mảng tổng tiền tố trong đó mỗi vị trí lưu trữ tổng số cây từ đầu đến chỉ mục đó. Điều này cho phép truy xuất bất kỳ tổng phân đoạn tiền tố nào ngay lập tức thay vì phải tính toán lại. 
2. Đối với mỗi truy vấn, hãy đọc điểm cuối A và B. Vì hướng đi không quan trọng, hãy sắp xếp lại chúng sao cho chỉ mục nhỏ hơn xuất hiện trước. Điều này đảm bảo chúng tôi luôn làm việc với khoảng thời gian tăng hợp lệ. 
3. Tính tổng số cây trong khoảng đó bằng mảng tiền tố. Tổng từ L đến R thu được bằng cách trừ giá trị tiền tố trước L khỏi giá trị tiền tố tại R. Điều này có tác dụng vì tổng tiền tố tích lũy tất cả các đóng góp cho đến một điểm và phép trừ sẽ loại bỏ mọi thứ trước khoảng. 
4. Thêm giá trị được tính toán vào tổng đang chạy, vì bài toán yêu cầu tổng trên tất cả các lần đi được kết hợp thay vì đầu ra trên mỗi truy vấn. 
5. Sau khi xử lý tất cả các truy vấn, xuất ra tổng số tích lũy. 

### Tại sao nó hoạt động 

Mảng tổng tiền tố mã hóa bất biến mà mỗi vị trí lưu trữ tổng đóng góp của tất cả các phần tử trước nó. Bất kỳ tổng phạm vi nào cũng có thể được biểu thị dưới dạng hiệu của hai giá trị tích lũy như vậy bởi vì tất cả các đóng góp chồng chéo sẽ bị loại trừ ngoại trừ những giá trị nằm hoàn toàn trong khoảng. Vì mỗi truy vấn được rút gọn thành một phép toán số học trong thời gian không đổi trên các giá trị được tính toán trước nên không xảy ra sự tính toán lại hoặc sự mơ hồ chồng chéo và mỗi khối đóng góp chính xác một lần cho mỗi truy vấn có chứa truy vấn đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    t = list(map(int, input().split()))
    
    prefix = [0] * (n + 1)
    for i in range(1, n + 1):
        prefix[i] = prefix[i - 1] + t[i - 1]
    
    total = 0
    for _ in range(m):
        a, b = map(int, input().split())
        if a > b:
            a, b = b, a
        total += prefix[b] - prefix[a - 1]
    
    print(total)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xây dựng một mảng tổng tiền tố trong đó chỉ số i biểu thị tổng số cây từ khối 1 đến khối i. Điều này được thực hiện trong một lần truyền tuyến tính duy nhất qua mảng đầu vào. 

Mỗi truy vấn sau đó được xử lý độc lập. Chi tiết triển khai tinh tế duy nhất là chuẩn hóa các điểm cuối, đảm bảo ranh giới bên trái không lớn hơn ranh giới bên phải. Phép trừ`prefix[b] - prefix[a - 1]`nắm bắt chính xác tổng của khoảng bao gồm mà không cần vòng lặp hoặc cắt. 

Một sai lầm phổ biến là quên rằng việc lập chỉ mục tiền tố yêu cầu một phần bù, vì tiền tố[0] đại diện cho một tổng trống. Đây là lý do tại sao chúng tôi truy cập`a - 1`an toàn ngay cả khi a bằng 1, vì tiền tố[0] được xác định là 0. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

đầu vào:```
4 2
2 1 6 1
1 2
2 4
```Việc xây dựng tiền tố và đánh giá truy vấn tiến hành như sau. 

| Bước | A | B | Chuẩn hóa (L, R) | tiền tố[L-1] | tiền tố[R] | Tổng phạm vi | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Truy vấn 1 | 1 | 2 | (1, 2) | 0 | 3 | 3 | 3 | 
| Truy vấn 2 | 2 | 4 | (2, 4) | 2 | 10 | 8 | 11 | 

Truy vấn đầu tiên tính tổng hai khối đầu tiên, cho ra 2 + 1 = 3. Truy vấn thứ hai tính tổng các khối từ 2 đến 4, cho ra 1 + 6 + 1 = 8. Tổng cộng là 11. 

Bây giờ hãy xem xét đầu vào thứ hai được thiết kế để kiểm tra các khoảng thời gian một khối và đảo ngược. 

đầu vào:```
5 3
3 0 4 2 1
5 2
1 1
3 3
```| Bước | A | B | Chuẩn hóa (L, R) | tiền tố[L-1] | tiền tố[R] | Tổng phạm vi | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Truy vấn 1 | 5 | 2 | (2, 5) | 3 | 10 | 7 | 7 | 
| Truy vấn 2 | 1 | 1 | (1, 1) | 0 | 3 | 3 | 10 | 
| Truy vấn 3 | 3 | 3 | (3, 3) | 4 | 7 | 4 | 14 | 

Dấu vết này cho thấy các khoảng đảo ngược được xử lý chính xác và các khoảng phần tử đơn vẫn đóng góp giá trị của chúng một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Một lượt xây dựng tổng tiền tố và mỗi truy vấn được trả lời theo thời gian không đổi | 
| Không gian | O(N) | Chỉ mảng tiền tố được lưu trữ ngoài đầu vào | 

Các ràng buộc cho phép tối đa 200 phần tử và 10.000 truy vấn. Giải pháp thực hiện một bước tiền xử lý tuyến tính duy nhất, sau đó thực hiện công việc trong thời gian không đổi cho mỗi truy vấn, nằm trong giới hạn thoải mái ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve_output(inp)).strip()

def solve_output(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    t = list(map(int, input().split()))
    prefix = [0] * (n + 1)
    for i in range(1, n + 1):
        prefix[i] = prefix[i - 1] + t[i - 1]
    total = 0
    for _ in range(m):
        a, b = map(int, input().split())
        if a > b:
            a, b = b, a
        total += prefix[b] - prefix[a - 1]
    return str(total)

# provided sample
assert solve_output("4 2\n2 1 6 1\n1 2\n2 4\n") == "11"

# single element range
assert solve_output("1 1\n7\n1 1\n") == "7"

# reversed interval
assert solve_output("3 1\n1 2 3\n3 1\n") == "6"

# all equal values
assert solve_output("5 2\n2 2 2 2 2\n1 5\n2 4\n") == "18"

# full coverage repeated
assert solve_output("4 3\n1 1 1 1\n1 4\n1 4\n1 4\n") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khối đơn | 7 | trường hợp ranh giới tối thiểu | 
| phạm vi đảo ngược | 6 | trật tự độc lập | 
| mảng thống nhất | 18 | tích lũy nhất quán | 
| lặp đi lặp lại đầy đủ | 12 | sử dụng tiền tố lặp đi lặp lại | 

## Vỏ cạnh 

Đầu vào một khối kiểm tra xem việc triển khai có xử lý chính xác việc lập chỉ mục tiền tố ở mức 0 hay không. Với đầu vào`1 1 / 7 / 1 1`, mảng tiền tố trở thành`[0, 7]`và truy vấn đánh giá là`prefix[1] - prefix[0]`, thu được 7. 

Một truy vấn đảo ngược như`3 1`xác minh rằng chuẩn hóa được áp dụng. Nếu không hoán đổi, một vòng lặp hoặc lát cắt đơn giản sẽ xử lý phạm vi không chính xác hoặc tạo ra một phân đoạn trống. Sau khi hoán đổi, khoảng trở nên hợp lệ và phép trừ tiền tố tạo ra tổng đầy đủ. 

Mảng thống nhất nhấn mạnh tính nhất quán tích lũy lặp đi lặp lại. Vì mọi khối đều có cùng một giá trị nên mỗi tổng phạm vi đều tỷ lệ thuận với độ dài của nó và phép trừ tiền tố sẽ bảo toàn chính xác giá trị này trên nhiều truy vấn.
