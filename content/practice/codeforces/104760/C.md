---
title: "CF 104760C - \u0414\u043e\u0440\u043e\u0433\u0430"
description: "Chúng ta được cung cấp một tập hợp các khoảng được đặt dọc theo một trục số. Mỗi khoảng đại diện cho một ngọn đèn chiếu sáng một đoạn đường, từ tọa độ bắt đầu đến tọa độ kết thúc."
date: "2026-06-28T22:00:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104760
codeforces_index: "C"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Qualification Contest"
rating: 0
weight: 104760
solve_time_s: 58
verified: true
draft: false
---

[CF 104760C - \u0414\u043e\u0440\u043e\u0433\u0430](https://codeforces.com/problemset/problem/104760/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các khoảng được đặt dọc theo một trục số. Mỗi khoảng đại diện cho một ngọn đèn chiếu sáng một đoạn đường, từ tọa độ bắt đầu đến tọa độ kết thúc. Nhiều đèn có thể chồng lên nhau và các phần của đường có thể bị che phủ bởi nhiều khoảng cách hoặc không có đèn nào cả. 

Nhiệm vụ là xác định tổng chiều dài của con đường được chiếu sáng bởi ít nhất một ngọn đèn. Nói cách khác, chúng ta cần tính độ dài hợp của tất cả các khoảng đã cho. 

Các ràng buộc cho phép lên tới 10.000 khoảng thời gian, với tọa độ từ âm đến dương một tỷ. Kích thước này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng kiểm tra từng điểm một trên đường. Việc rời rạc hóa đơn giản trong phạm vi tọa độ sẽ yêu cầu lặp lại tới 2 tỷ vị trí, vượt xa giới hạn khả thi. Ngay cả việc lưu trữ mọi tọa độ có thể một cách rõ ràng cũng không thể thực hiện được cả về thời gian và bộ nhớ. 

Một vấn đề tế nhị hơn phát sinh với việc xử lý chồng chéo. Ví dụ: nếu các khoảng được lồng nhau hoặc chồng chéo nhiều, chỉ cần tính tổng độ dài của chúng sẽ vượt quá các vùng được chia sẻ. Xem xét khoảng thời gian`[0, 10]`Và`[5, 15]`. Tổng độ dài cho 10 + 10 = 20, nhưng kết hợp đúng là`[0, 15]`, có độ dài 15. Bất kỳ giải pháp đúng nào cũng phải tránh tính hai lần các phân đoạn chồng chéo. 

Các trường hợp biên thường phá vỡ quá trình triển khai đơn giản bao gồm các khoảng hoàn toàn rời rạc, các khoảng được lồng hoàn toàn và các khoảng chỉ chạm vào điểm cuối. Ví dụ,`[0, 5]`Và`[5, 10]`sẽ đóng góp tổng độ dài là 10 nếu điểm cuối được coi là phạm vi bao phủ liên tục, đây là cách giải thích tiêu chuẩn trong vấn đề này. Việc triển khai "chỉ hợp nhất nếu chồng chéo là nghiêm ngặt" có thể coi các khoảng thời gian chạm là riêng biệt một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là mô phỏng phạm vi phủ sóng trên một dòng số chi tiết. Người ta có thể tưởng tượng việc đánh dấu mọi điểm nguyên nằm trong bất kỳ khoảng nào và sau đó đếm xem có bao nhiêu điểm được đánh dấu. Mặc dù đơn giản về mặt khái niệm nhưng điều này không thành công ngay lập tức vì tọa độ trải dài từ -10^9 đến 10^9. Ngay cả khi chúng tôi nén tọa độ, việc đánh dấu hoặc quét từng điểm vẫn quá chậm vì mỗi khoảng có thể rất lớn. 

Một ý tưởng táo bạo khác là so sánh từng cặp khoảng thời gian và hợp nhất các khoảng chồng chéo liên tục cho đến khi ổn định. Điều này dẫn đến quy trình O(N^2) trong trường hợp xấu nhất, vì mỗi bước hợp nhất có thể yêu cầu quét và cập nhật nhiều khoảng thời gian và có thể cần phải thực hiện lặp lại. 

Quan sát quan trọng là chúng ta thực sự không cần biết phạm vi cấp điểm. Chúng ta chỉ cần biết mức độ phù hợp thay đổi từ “không được khám phá” sang “được che phủ” và ngược lại. Sau khi các khoảng được sắp xếp theo tọa độ bắt đầu, các phần chồng chéo sẽ có cấu trúc: khi chúng tôi xử lý các khoảng theo thứ tự, chúng tôi chỉ cần duy trì điểm cuối bên phải xa nhất được thấy cho đến nay. Bất kỳ khoảng mới nào cũng sẽ mở rộng vùng được bao phủ hiện tại hoặc bắt đầu một vùng rời rạc mới. 

Điều này làm giảm vấn đề thành một lần quét cổ điển trong các khoảng thời gian được sắp xếp, duy trì phân khúc đang hoạt động hiện tại và tích lũy đóng góp của nó bất cứ khi nào xuất hiện khoảng trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đánh dấu lực lượng vũ phu | O(R) trong đó R là phạm vi tọa độ | O(R) | Không thể | 
| Hợp nhất theo cặp | O(N^2) | O(N) | Quá chậm | 
| Sắp xếp + quét | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoảng và sắp xếp chúng theo tọa độ bắt đầu của chúng. Việc sắp xếp đảm bảo rằng khi chúng ta di chuyển từ trái sang phải, chúng ta sẽ không bao giờ xem lại những khoảng trống chưa được phát hiện trước đó một cách không chính xác. 
2. Khởi tạo phân đoạn đang chạy bằng khoảng đầu tiên. Phân đoạn này thể hiện sự kết hợp hiện tại của các khoảng chồng chéo mà chúng tôi đang xây dựng. 
3. Lặp lại từng khoảng thời gian còn lại theo thứ tự được sắp xếp. 
4. Đối với mỗi khoảng thời gian, hãy so sánh phần đầu của nó với phần cuối của đoạn hiện tại. Nếu điểm bắt đầu nhỏ hơn hoặc bằng điểm cuối hiện tại, thì các khoảng sẽ chồng lên nhau hoặc chạm vào nhau, do đó chúng ta sẽ mở rộng điểm cuối của đoạn hiện tại đến mức tối đa của cả hai đầu. Điều này duy trì tính liên tục của vùng phủ sóng mà không cần tính hai lần. 
5. Nếu phần bắt đầu lớn hơn phần cuối hiện tại thì có một khoảng cách giữa đoạn được phủ hiện tại và khoảng mới. Trong trường hợp này, hãy thêm độ dài của đoạn hiện tại vào câu trả lời, sau đó bắt đầu một đoạn mới từ khoảng hiện tại. 
6. Sau khi xử lý tất cả các khoảng, hãy thêm phân đoạn hoạt động cuối cùng vào câu trả lời. 

Ý tưởng chính là chúng tôi chỉ hoàn thiện một phân khúc khi chúng tôi chắc chắn rằng không có khoảng thời gian nào trong tương lai có thể kéo dài phân khúc đó, điều này xảy ra chính xác khi chúng tôi phát hiện ra khoảng trống. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì một bất biến: phân đoạn hiện tại thể hiện chính xác sự kết hợp của tất cả các khoảng được xử lý cho đến nay giao nhau hoặc tạo thành một chuỗi được kết nối thông qua các phần chồng chéo. Bởi vì các khoảng thời gian được xử lý theo thứ tự bắt đầu tăng dần nên bất kỳ khoảng thời gian nào trong tương lai chỉ có thể mở rộng phân đoạn này hoặc bắt đầu ngay sau phân đoạn đó. Do đó, khi chúng tôi gặp một khoảng mới bắt đầu sau phần cuối hiện tại, phân đoạn hiện tại được xác định đầy đủ và không thể bị ảnh hưởng bởi bất kỳ dữ liệu nào trong tương lai, do đó, độ dài của nó có thể được thêm vào kết quả cuối cùng một cách an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    intervals = []
    for _ in range(n):
        a, b = map(int, input().split())
        intervals.append((a, b))

    intervals.sort()

    cur_l, cur_r = intervals[0]
    total = 0

    for l, r in intervals[1:]:
        if l <= cur_r:
            if r > cur_r:
                cur_r = r
        else:
            total += cur_r - cur_l
            cur_l, cur_r = l, r

    total += cur_r - cur_l
    print(total)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc tất cả các khoảng và sắp xếp chúng, điều này rất cần thiết để đảm bảo rằng các phần trùng lặp xuất hiện liên tiếp. Các biến`cur_l`Và`cur_r`theo dõi phân đoạn đã hợp nhất hiện đang hoạt động. Khi một khoảng mới chồng lên nhau, chỉ ranh giới bên phải mới có thể mở rộng, đó là lý do tại sao chúng tôi lấy mức tối đa. Khi tìm thấy khoảng trống, độ dài đoạn hiện tại sẽ được hoàn thiện và thêm vào`total`, và chúng tôi khởi động lại phân đoạn. 

Một chi tiết tinh tế là chúng tôi không bao giờ thêm đóng góp một phần trong quá trình xử lý chồng chéo. Điều này tránh hoàn toàn việc tính hai lần, vì mỗi phân đoạn được tính chính xác một lần khi nó được đóng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
30 40
5 8
3 22
40 42
0 10
```Các khoảng được sắp xếp:`(0,10), (3,22), (5,8), (30,40), (40,42)`| Bước | Khoảng thời gian | Phân khúc hiện tại | Hành động | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | (0,10) | (0,10) | khởi tạo | 0 | 
| 2 | (3,22) | (0,22) | mở rộng | 0 | 
| 3 | (5,8) | (0,22) | không thay đổi | 0 | 
| 4 | (30,40) | hoàn thiện (0,22) | thêm 22 | 22 | 
| 5 | (30,40) | (30,40) | bắt đầu mới | 22 | 
| 6 | (40,42) | (30,42) | mở rộng | 22 | 
| kết thúc | - | hoàn thiện (30,42) | thêm 12 | 34 | 

Dấu vết này cho thấy các khoảng chồng chéo thu gọn thành một đoạn liên tục duy nhất như thế nào, trong khi các phần rời rạc được tích lũy riêng biệt. 

### Ví dụ 2 

đầu vào:```
3
1 3
2 5
6 8
```Các khoảng được sắp xếp:`(1,3), (2,5), (6,8)`| Bước | Khoảng thời gian | Phân khúc hiện tại | Hành động | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | (1,3) | (1,3) | khởi tạo | 0 | 
| 2 | (2,5) | (1,5) | mở rộng | 0 | 
| 3 | (6,8) | hoàn thiện (1,5) | thêm 4 | 4 | 
| kết thúc | - | hoàn thiện (6,8) | thêm 2 | 6 | 

Ví dụ này nêu bật logic phát hiện khoảng trống kích hoạt quá trình hoàn thiện phân đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp chiếm ưu thế; quét tuyến tính đơn sau đó | 
| Không gian | O(N) | Lưu trữ các khoảng thời gian | 

Các ràng buộc cho phép lên tới 10.000 khoảng thời gian, do đó việc sắp xếp và quét tuyến tính dễ dàng đủ nhanh. Thuật toán tránh mọi sự phụ thuộc vào cường độ tọa độ, điều này rất quan trọng với phạm vi lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    intervals = []
    for _ in range(n):
        a, b = map(int, input().split())
        intervals.append((a, b))

    intervals.sort()

    cur_l, cur_r = intervals[0]
    total = 0

    for l, r in intervals[1:]:
        if l <= cur_r:
            if r > cur_r:
                cur_r = r
        else:
            total += cur_r - cur_l
            cur_l, cur_r = l, r

    total += cur_r - cur_l
    return str(total).strip()

# provided sample
assert run("""5
30 40
5 8
3 22
40 42
0 10
""") == "34"

# disjoint intervals
assert run("""2
0 1
3 4
""") == "2"

# fully nested intervals
assert run("""3
0 10
2 5
3 7
""") == "10"

# touching intervals
assert run("""3
0 5
5 10
10 15
""") == "15"

# single interval
assert run("""1
-5 5
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng rời rạc | 2 | xử lý khoảng cách | 
| khoảng lồng nhau | 10 | không đếm thừa | 
| khoảng cách chạm | 15 | hợp nhất điểm cuối | 
| khoảng đơn | 10 | trường hợp cơ sở | 

## Vỏ cạnh 

Đối với các khoảng rời rạc như`[0,1]`Và`[3,4]`, thuật toán xử lý khoảng đầu tiên, sau đó phát hiện`3 > 1`và hoàn thành phân đoạn đầu tiên trước khi bắt đầu phân đoạn mới. Điều này đảm bảo không có sự hợp nhất nhân tạo nào xảy ra trên các khoảng trống. 

Đối với các khoảng lồng nhau như`[0,10]`,`[2,5]`,`[3,7]`, thời điểm kết thúc hoạt động chỉ tăng lên 10 và không xảy ra tình trạng đóng sớm vì tất cả các lần khởi động đều nằm trong phạm vi hiện tại. Tính bất biến đó`cur_r`là điểm cuối bên phải tối đa của tất cả các khoảng chồng chéo đảm bảo tính chính xác. 

Đối với các khoảng thời gian chạm như`[0,5]`Và`[5,10]`, điều kiện`l <= cur_r`coi chúng như được kết nối, vì vậy chúng hợp nhất thành`[0,10]`. Điều này tránh được việc đếm thiếu do quy ước phân tách ranh giới trong các vấn đề bao phủ liên tục.
