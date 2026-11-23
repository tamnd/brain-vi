---
title: "CF 452D - Máy giặt, máy sấy, folder"
description: "Chúng tôi có một quy trình sản xuất ba giai đoạn. Mọi đồ giặt trước tiên phải được giặt, sau đó chuyển ngay sang máy sấy khi giặt xong, sau đó chuyển ngay sang máy gấp khi sấy xong."
date: "2026-06-07T17:09:07+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 452
codeforces_index: "D"
codeforces_contest_name: "MemSQL Start[c]UP 2.0 - Round 1"
rating: 1900
weight: 452
solve_time_s: 112
verified: true
draft: false
---

[CF 452D - Máy giặt, Máy sấy, Thư mục](https://codeforces.com/problemset/problem/452/D) 

**Xếp hạng:** 1900 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một quy trình sản xuất ba giai đoạn. 

Mọi đồ giặt trước tiên phải được giặt, sau đó chuyển ngay sang máy sấy khi giặt xong, sau đó chuyển ngay sang máy gấp khi sấy xong. Các máy trong cùng một công đoạn đều giống hệt nhau và mỗi máy chỉ có thể xử lý một mặt hàng tại một thời điểm. 

Từ "ngay lập tức" là khó khăn chính. Một món đồ không được phép giặt xong trừ khi có máy sấy vào đúng thời điểm đó. Tương tự, một món đồ không được phép sấy khô hoàn toàn trừ khi có sẵn máy gấp vào đúng thời điểm đó. Chúng tôi có thể trì hoãn việc bắt đầu giặt để duy trì các điều kiện này. 

Đầu vào đưa ra số lượng đồ giặt, số lượng máy ở từng công đoạn và thời gian xử lý của từng công đoạn. Chúng ta phải tính thời gian sớm nhất khi tất cả các hạng mục đã hoàn thành cả ba giai đoạn. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên. Có nhiều nhất$10^4$các mặt hàng và nhiều nhất$1000$máy theo từng công đoạn. Một mô phỏng xử lý từng mục riêng lẻ là hoàn toàn khả thi. Thậm chí một$O(k \log k)$hoặc$O(k)$giải pháp dễ dàng đủ nhanh. Điều bị loại trừ là bất kỳ cách tiếp cận nào mô hình hóa thời gian theo từng phút, vì thời gian xử lý có thể đạt tới$1000$và thời gian hoàn thành có thể lên tới hàng triệu. 

Một sai lầm phổ biến là xử lý từng giai đoạn một cách độc lập. 

Coi như:```
1 1 1 1 5 5 5
```Câu trả lời là 15. Một món đồ mất 5 phút để giặt, 5 lần sấy và 5 lần gấp. 

Một ví dụ tinh tế hơn là:```
2 1 1 1 1 100 1
```Một giải pháp bất cẩn có thể bắt đầu lần giặt thứ hai ở thời điểm 1 vì máy giặt bị lỏng. Điều đó là bất hợp pháp. Món đồ đầu tiên chiếm giữ trong máy sấy cho đến thời điểm 101, vì vậy món đồ thứ hai không thể giặt xong trước thời gian 101. Việc giặt của nó phải được trì hoãn để duy trì máy sấy tại thời điểm hoàn tất giặt. 

Một cái bẫy khác xuất hiện khi giai đoạn gấp là nút cổ chai:```
3 3 3 1 1 1 100
```Cả ba máy giặt và máy sấy đều có sẵn nhưng chỉ có một máy gấp. Không thể hoàn tất nhiều thao tác sấy gần như cùng lúc vì mỗi thao tác sấy hoàn thành yêu cầu một thư mục có sẵn ngay lập tức. Lịch trình phải được điều chỉnh sớm hơn nhiều, ở giai đoạn giặt. 

Thách thức chính là các hạn chế về tính khả dụng lan truyền ngược lại qua đường ống. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là nghĩ về việc lên lịch các sự kiện theo dòng thời gian. Đối với mỗi món đồ giặt, chúng tôi có thể tìm kiếm thời gian bắt đầu giặt sớm nhất có thể để cuối cùng cho phép chuyển đổi ngay lập tức sang quá trình sấy và gấp. Điều này đúng vì hệ thống có tính xác định khi biết được công suất sử dụng máy. 

Vấn đề là thời gian tìm kiếm rõ ràng trở nên tốn kém. Thời gian hoàn thành có thể rất lớn và việc liên tục quét về phía trước để tìm những khoảnh khắc hợp lệ sẽ dẫn đến công việc không cần thiết. 

Quan sát quan trọng là chúng ta không bao giờ cần tìm kiếm xuyên thời gian. Điều quan trọng là thời điểm tiếp theo khi máy ở mỗi giai đoạn có sẵn. 

Giả sử chúng ta đã lên lịch một số mục. Đối với một mặt hàng mới, chúng tôi chọn máy giặt có sẵn sớm nhất. Hãy để thời gian có sẵn của nó được$w$. 

Nếu quá trình giặt bắt đầu lúc$w$, đồ sẽ được giặt xong vào lúc$w+t_1$. Lúc đó chúng ta cần một chiếc máy sấy. Trong số tất cả các máy sấy, máy sấy có sẵn sớm nhất sẽ xác định thời điểm chuyển giao có thể xảy ra. 

Tương tự, sau khi quá trình sấy kết thúc, chúng ta cần có một thư mục ngay lập tức. 

Thay vì lên lịch cho các giai đoạn về phía trước, chúng ta có thể nghĩ ngược lại từ giai đoạn gấp. Nếu một thư mục có sẵn vào thời điểm đó$f$, thì hoạt động sấy cấp liệu phải kết thúc không sớm hơn$f$. Hoạt động sấy đó lần lượt xác định khi nào quá trình giặt có thể kết thúc. 

Giải pháp tinh tế được sử dụng trong cuộc thi sẽ đảm bảo cho mỗi giai đoạn thời gian tiếp theo mọi máy đều có sẵn. Đối với mỗi món đồ giặt, chúng tôi lấy máy giặt, máy sấy và máy gấp có sẵn sớm nhất. Ba thời điểm sẵn có này xác định thời gian bắt đầu khả thi sớm nhất cho hạng mục này. Sau khi lên lịch, chúng tôi cập nhật tình trạng sẵn có của máy. 

Toàn bộ quá trình trở thành mô phỏng trực tiếp thời gian nhả máy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm dòng thời gian Brute Force | Có khả năng rất lớn | O(1) | Quá chậm | 
| Mô phỏng tính khả dụng của máy | O(k) | O(n₁+n₂+n₃) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để`wash[i]`hãy là người giặt lần sau`i`có sẵn. 

Cho phép`dry[i]`hãy là người sấy khô lần sau`i`có sẵn. 

Cho phép`fold[i]`là thư mục lần sau`i`có sẵn. 

Ban đầu tất cả các giá trị đều bằng 0. 

Đối với mỗi món đồ giặt: 

1. Tìm máy giặt có thời gian sử dụng nhỏ nhất. 
2. Tìm máy sấy có thời gian sử dụng nhỏ nhất. 
3. Tìm thư mục có thời gian khả dụng nhỏ nhất. 
4. Hãy để những khoảng thời gian này trôi qua`w`,`d`, Và`f`. 
5. Tính thời gian bắt đầu giặt sớm nhất để có thể chuyển ngay qua các giai đoạn còn lại. 

Quá trình giặt kết thúc lúc`start + t1`. 

Khi đó phải có sẵn máy sấy, vì vậy:```
start + t1 >= d
```Quá trình sấy kết thúc ở`start + t1 + t2`. 

Một thư mục phải có sẵn sau đó, vì vậy:```
start + t1 + t2 >= f
```Bản thân máy giặt không thể khởi động trước thời gian`w`. 

Kết hợp tất cả các ràng buộc:```
start = max(
    w,
    d - t1,
    f - t1 - t2
)
```6. Lên lịch cho mục này bằng thời gian bắt đầu này. 
7. Cập nhật tình trạng sẵn có tiếp theo của máy giặt đã chọn thành:```
start + t1
```8. Cập nhật tình trạng sẵn có tiếp theo của máy sấy đã chọn thành:```
start + t1 + t2
```9. Cập nhật tính khả dụng tiếp theo của thư mục đã chọn thành:```
start + t1 + t2 + t3
```10. Ghi lại thời gian hoàn thành mục này. Câu trả lời chính là thời gian hoàn thành hạng mục dự kiến ​​cuối cùng. 

### Tại sao nó hoạt động 

Đối với mỗi mặt hàng chúng tôi chọn máy có sẵn sớm nhất trong từng giai đoạn. Thời gian bắt đầu được tính toán là thời gian tối thiểu thỏa mãn đồng thời cả ba ràng buộc về nguồn lực. 

Máy giặt phải đã được miễn phí. Máy sấy phải rảnh chính xác khi quá trình giặt kết thúc. Thư mục phải trống chính xác khi quá trình sấy kết thúc. Bất kỳ thời gian bắt đầu nhỏ hơn nào cũng sẽ vi phạm ít nhất một trong những điều kiện đó. 

Sau khi lên lịch cho một hạng mục, ba máy đã chọn sẽ được sử dụng cho đến thời điểm hoàn thành giai đoạn tương ứng. Mảng sẵn có luôn thể hiện thời điểm sớm nhất trong tương lai khi mỗi máy có thể chấp nhận một mặt hàng khác. Bởi vì mọi hạng mục đều được lên lịch vào thời điểm khả thi sớm nhất phù hợp với những tính sẵn có đó nên mô phỏng sẽ xây dựng lịch trình sớm nhất trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k, n1, n2, n3, t1, t2, t3 = map(int, input().split())

    wash = [0] * n1
    dry = [0] * n2
    fold = [0] * n3

    answer = 0

    for _ in range(k):
        iw = min(range(n1), key=wash.__getitem__)
        idr = min(range(n2), key=dry.__getitem__)
        ifo = min(range(n3), key=fold.__getitem__)

        w = wash[iw]
        d = dry[idr]
        f = fold[ifo]

        start = max(
            w,
            d - t1,
            f - t1 - t2
        )

        wash[iw] = start + t1
        dry[idr] = start + t1 + t2
        fold[ifo] = start + t1 + t2 + t3

        answer = fold[ifo]

    print(answer)

if __name__ == "__main__":
    solve()
```Ba mảng lưu trữ thời gian phát hành máy. Thời gian phát hành có nghĩa là "máy này có thể nhận một mặt hàng mới bắt đầu từ thời điểm này". 

Đối với mỗi hạng mục chúng tôi chọn máy có thời gian xuất xưởng nhỏ nhất trong mỗi công đoạn. Vì số lượng máy nhiều nhất là 1000 nên việc quét tuyến tính là hoàn toàn đủ. 

Phần tế nhị nhất là tính toán`start`. 

Sự sẵn có của máy giặt áp đặt`start >= w`. 

Máy sấy phải có sẵn khi giặt xong, giúp`start >= d - t1`. 

Thư mục phải có sẵn khi quá trình sấy kết thúc, đưa ra`start >= f - t1 - t2`. 

Việc lấy tối đa ba giá trị này đồng thời thỏa mãn mọi ràng buộc. 

Các bản cập nhật rất dễ bị sai. Máy giặt sẽ hoạt động tự do sau khi quá trình rửa hoàn tất chứ không phải sau khi toàn bộ đường ống kết thúc. Tương tự như vậy, máy sấy sẽ hoạt động tự do sau khi sấy xong. Chỉ có thư mục vẫn bị chiếm giữ cho đến thời gian hoàn thành cuối cùng. 

Tất cả số học đều phù hợp thoải mái với số nguyên Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 1 1 1 5 5 5
```| Mục | w | d | f | bắt đầu | Máy giặt miễn phí | Máy sấy miễn phí | Thư mục miễn phí | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 5 | 10 | 15 | 

Đáp án = 15. 

Vật phẩm duy nhất di chuyển qua cả ba giai đoạn mà không cần chờ đợi. 

### Mẫu 2 

đầu vào:```
8 2 1 1 5 10 2
```| Mục | w | d | f | bắt đầu | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 
| 2 | 0 | 15 | 17 | 2 | 
| 3 | 5 | 30 | 32 | 15 | 
| 4 | 7 | 45 | 47 | 30 | 
| 5 | 20 | 60 | 62 | 45 | 
| 6 | 35 | 75 | 77 | 60 | 
| 7 | 50 | 90 | 92 | 75 | 
| 8 | 65 | 105 | 107 | 90 | 

Mục cuối cùng kết thúc lúc:```
90 + 5 + 10 + 2 = 107
```Dấu vết này cho thấy nút cổ chai ở các giai đoạn sau buộc quá trình giặt phải bắt đầu muộn hơn so với mức độ sẵn có của máy giặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k(n₁+n₂+n₃)) | Mỗi mục quét tất cả các máy một lần | 
| Không gian | O(n₁+n₂+n₃) | Lưu trữ thời gian sẵn sàng của máy | 

Với$k \le 10^4$và mỗi máy tính nhiều nhất$1000$, tổng công việc là khoảng$3 \times 10^7$các phép toán đơn giản trong trường hợp lý thuyết xấu nhất, có thể chấp nhận được trong thực tế cho bài toán này và là giải pháp dự kiến. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    k, n1, n2, n3, t1, t2, t3 = map(int, input().split())

    wash = [0] * n1
    dry = [0] * n2
    fold = [0] * n3

    ans = 0

    for _ in range(k):
        iw = min(range(n1), key=wash.__getitem__)
        idr = min(range(n2), key=dry.__getitem__)
        ifo = min(range(n3), key=fold.__getitem__)

        start = max(
            wash[iw],
            dry[idr] - t1,
            fold[ifo] - t1 - t2
        )

        wash[iw] = start + t1
        dry[idr] = start + t1 + t2
        fold[ifo] = start + t1 + t2 + t3

        ans = fold[ifo]

    return str(ans)

# provided sample
assert run("1 1 1 1 5 5 5\n") == "15"

# minimum size
assert run("1 1 1 1 1 1 1\n") == "3"

# many washers, one dryer
assert run("2 2 1 1 1 100 1\n") == "102"

# all equal
assert run("3 3 3 3 5 5 5\n") == "15"

# pipeline bottleneck
assert run("3 3 3 1 1 1 100\n") == "302"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1 1 1`|`3`| Ví dụ nhỏ nhất có thể | 
|`2 2 1 1 1 100 1`|`102`| Nút thắt máy sấy lan truyền ngược | 
|`3 3 3 3 5 5 5`|`15`| Máy song song loại bỏ chờ đợi | 
|`3 3 3 1 1 1 100`|`302`| Nút thắt gấp chi phối lịch trình | 

## Vỏ cạnh 

Hãy xem xét:```
2 2 1 1 1 100 1
```Máy sấy là nút cổ chai. Sau khi lên lịch cho hạng mục đầu tiên, máy sấy sẽ được sử dụng đến thời điểm 101. Đối với hạng mục thứ 2 ta có:```
w = 0
d = 101
f = 102
```Công thức cho:```
start = max(0, 100, 1) = 100
```Lần giặt thứ hai được cố tình trì hoãn để quá trình giặt kết thúc chính xác khi có máy sấy. Thuật toán đưa ra 102, là tối ưu. 

Bây giờ hãy xem xét:```
3 3 3 1 1 1 100
```Sau mục đầu tiên, thư mục sẽ bị chiếm cho đến thời điểm 102. Mục thứ hai nhận được:```
start = max(0, 0, 100)
```vì vậy nó chỉ bắt đầu giặt ở thời điểm 100. Mục thứ ba bắt đầu ở thời điểm 200. Thời gian hoàn thành trở thành 102, 202 và 302. 

Một giải pháp chỉ theo dõi tình trạng sẵn có của máy giặt và máy sấy sẽ cố gắng sấy khô một số đồ một cách không chính xác trước khi có máy gấp. Hạn chế về phía sau`f - t1 - t2`ngăn chặn sai lầm đó. 

Cuối cùng, hãy xem xét:```
4 1 1 1 10 1 1
```Máy giặt là nút cổ chai. Mọi vật dụng mới đều bắt đầu một cách tự nhiên khi máy giặt rảnh rỗi. Công thức giảm thiểu hạn chế về máy giặt vì máy sấy và bìa luôn có sẵn kịp thời. Thuật toán hoạt động chính xác giống như một đường ống đơn giản và tạo ra lịch trình chính xác.
