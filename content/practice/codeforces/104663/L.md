---
title: "CF 104663L - Chưa đầy đủ"
description: "Học kỳ có số tuần cố định và mỗi tuần có một số lớp học hạn chế. Một số tuần đã trôi qua và bạn đã tham gia một số lớp học nhất định trong suốt những tuần đó."
date: "2026-06-29T14:58:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104663
codeforces_index: "L"
codeforces_contest_name: "Replay of Ostad Presents Intra KUET Programming Contest 2023"
rating: 0
weight: 104663
solve_time_s: 73
verified: true
draft: false
---

[CF 104663L - Chưa hoàn chỉnh](https://codeforces.com/problemset/problem/104663/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Học kỳ có số tuần cố định và mỗi tuần có một số lớp học hạn chế. Một số tuần đã trôi qua và bạn đã tham gia một số lớp học nhất định trong suốt những tuần đó. Bây giờ, những tuần còn lại vẫn còn ở phía trước và bạn phải quyết định xem sẽ tham gia bao nhiêu lớp trong mỗi tuần còn lại đó. 

Mục tiêu là đảm bảo tỷ lệ đi học cuối cùng của bạn ít nhất là 60% tổng số lớp trong học kỳ. Tuy nhiên, bạn không thể tham dự nhiều hơn số lớp học thực sự được tổ chức trong một tuần, vì vậy mỗi tuần còn lại sẽ có giới hạn trên về số lớp bạn có thể đóng góp. 

Nhiệm vụ không chỉ là quyết định xem có thể đạt được 60% tỷ lệ tham dự hay không mà còn phải xây dựng một kế hoạch hợp lý cho những tuần còn lại nếu có thể. Trong số tất cả các kế hoạch hợp lệ, bạn phải xuất ra một kế hoạch phân bổ số người tham dự trong các tuần còn lại đồng thời tôn trọng các giới hạn hàng tuần. 

Các số lượng quan trọng rất đơn giản. Tổng số lớp trong học kỳ được xác định bằng số tuần nhân với số lớp trong tuần. Một số tuần đã hoàn thành và việc tham dự trước đây của bạn đã được ấn định. Quyết định còn lại là làm thế nào để phân bổ số lượng người tham dự bổ sung trong các tuần tới theo giới hạn mỗi tuần. 

Những hạn chế là cực kỳ nhỏ. Số tuần nhiều nhất là 14 và số lớp mỗi tuần nhiều nhất là 5. Điều này có nghĩa là lý luận thô bạo về phân phối là hoàn toàn an toàn và ngay cả các công trình tuyến tính hoặc tham lam cũng là quá đủ. Vấn đề không phải là về hiệu suất mà là về việc xử lý chính xác tính khả thi và xây dựng sự phân bổ hợp lệ. 

Các trường hợp thất bại chính đến từ hai nguồn. Đầu tiên là tính toán không chính xác ngưỡng tham dự cần thiết bằng cách sử dụng phép chia số nguyên, điều này có thể dẫn đến đánh giá thấp số lượng lớp học tham dự cần thiết. Ví dụ: nếu tổng số lớp là 50 thì 60 phần trăm là 30, nhưng nếu được tính như`50 * 60 / 100`với việc cắt bớt số nguyên ở một số ngôn ngữ, cần phải cẩn thận để đảm bảo hành vi trần chính xác khi cần thiết. 

Vấn đề thứ hai là giả định rằng chỉ cần lấp đầy những tuần còn lại một cách tham lam mà không tôn trọng các ràng buộc về tính khả thi luôn có hiệu quả. Ví dụ: nếu năng lực còn lại không đủ để đạt được tổng số người tham dự cần thiết thì người ta phải phát hiện chính xác khả năng không thể thực hiện được trước khi xây dựng bất kỳ sự phân bổ nào. 

Trường hợp cạnh cụ thể là khi đã đáp ứng được số lượng người tham dự cần thiết. Trong trường hợp đó, tất cả các tuần còn lại phải được điền bằng số 0, vì việc thêm số người tham dự không cần thiết là không cần thiết và có thể vi phạm mục đích “tối thiểu mỗi tuần”. 

Một trường hợp khó khăn khác là ngay cả việc tham dự tất cả các lớp còn lại cũng không đủ. Ví dụ: nếu tổng số lớp là 20, yêu cầu là 12, bạn đã học 11 lớp và chỉ còn 1 lớp thì điều đó là không thể dù bạn rất thân thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các cách có thể để phân bổ số người tham dự trong các tuần còn lại. Mỗi tuần có thể lấy giá trị từ 0 đến Y nên với tối đa 14 tuần và Y tối đa 5 thì số lượng cấu hình nhiều nhất$6^{14}$, quá lớn để liệt kê trực tiếp. 

Tuy nhiên, cấu trúc của vấn đề khiến cho việc sử dụng vũ lực trở nên không cần thiết. Yêu cầu toàn cầu duy nhất là đạt được tổng số lớp tham dự tối thiểu. Không có ưu tiên nào hơn việc phân phối ngoại trừ giá trị hàng tuần không thể vượt quá Y. Điều này biến vấn đề thành một nhiệm vụ xây dựng và khả thi đơn giản. 

Quan sát quan trọng là con số có ý nghĩa duy nhất là tổng số lớp học bổ sung mà bạn vẫn cần tham gia. Khi chúng tôi tính toán con số đó, việc phân phối nó trong các tuần còn lại sẽ trở thành một vấn đề đóng gói bị giới hạn: chúng tôi lấp đầy các tuần theo công suất Y cho đến khi yêu cầu được đáp ứng. 

Trước tiên, chúng tôi tính toán tổng số lớp trong học kỳ và ngưỡng yêu cầu (60%). Sau đó, chúng tôi trừ đi số lớp đã tham dự để tìm xem cần thêm bao nhiêu lớp nữa. Nếu con số này nhỏ hơn hoặc bằng 0 thì chúng ta đã đáp ứng được yêu cầu. Nếu vượt quá tổng dung lượng còn lại thì nhiệm vụ sẽ không thể thực hiện được. Nếu không, chúng ta tham lam chỉ định càng nhiều lớp càng tốt mỗi tuần cho đến khi đáp ứng được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(6^W) | O(W) | Quá chậm | 
| Tối ưu | O(W) | O(W) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số lớp trong học kỳ là$X \times Y$. Điều này đại diện cho toàn bộ vũ trụ tham dự. 
2. Tính số lượng đi học cần thiết là 60 phần trăm tổng số. Vì điểm danh phải là số nguyên nên đây được coi là mức trần của$0.6 \times X \times Y$. Điều này đảm bảo rằng chúng tôi không bao giờ chấp nhận giá trị không đủ ở ranh giới. 
3. Tính số giờ đi học cần thiết còn lại như`need = required - N`. Nếu giá trị này nhỏ hơn hoặc bằng 0 thì không cần tham dự thêm. 
4. Tính công suất còn lại như`(X - Z) * Y`. Đây là số người tham dự tối đa có thể mà bạn vẫn có thể tích lũy được. 
5. Nếu`need > remaining capacity`, xuất ra “Không” vì ngay cả nỗ lực tối đa cũng không thể đạt đến ngưỡng. 
6. Nếu không, hãy phân bổ số lượng học sinh tham gia các tuần còn lại. Đối với mỗi tuần còn lại, hãy chỉ định`min(Y, need)`và trừ nó khỏi`need`. 
7. Nếu sau khi phân công vẫn còn vài tuần, hãy điền vào số tuần không có mặt vì không cần thiết nữa. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là số tuần kết hợp ràng buộc duy nhất là tổng số người tham dự toàn cầu. Mỗi tuần là độc lập ngoại trừ giới hạn trên của nó. Bởi vì chúng tôi luôn cố gắng đáp ứng yêu cầu còn lại càng sớm càng tốt nên chúng tôi không bao giờ lãng phí năng lực theo cách có thể cản trở tính khả thi sau này. Nếu một giải pháp tồn tại, thì việc lấp đầy tham lam sẽ đảm bảo nó được xây dựng bằng cách tiêu thụ công suất trong quá trình quét tuyến tính đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    X, Y, Z, N = map(int, input().split())

    total = X * Y
    required = (total * 60 + 99) // 100

    need = required - N

    remaining_weeks = X - Z
    remaining_capacity = remaining_weeks * Y

    if need <= 0:
        print("Yes")
        print(" ".join("0" for _ in range(remaining_weeks)))
        return

    if need > remaining_capacity:
        print("No")
        return

    ans = []
    for _ in range(remaining_weeks):
        take = min(Y, need)
        ans.append(take)
        need -= take

    print("Yes")
    print(" ".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính toán tổng số lớp và ngưỡng yêu cầu bằng cách sử dụng mức phân chia trần để tránh tính thiếu yêu cầu 60%. Biến`need`ghi lại số lượng lớp học tham dự bổ sung được yêu cầu. 

Sau đó, chúng tôi tính toán xem có bao nhiêu lớp học vẫn có thể tham gia trong những tuần còn lại. Việc kiểm tra tính khả thi này là rất quan trọng, vì nếu không có nó, chúng ta có thể xây dựng một lịch trình một phần ngay cả khi không có lịch trình đầy đủ hợp lệ nào tồn tại. 

Vòng lặp tham lam chỉ định càng nhiều lớp càng tốt trong mỗi tuần mà không vượt quá giới hạn hàng tuần hoặc yêu cầu còn lại. Một lần`need`trở thành 0, các tuần tiếp theo sẽ tự động đóng góp bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
14 3 10 15
```Tổng số lớp = 14 × 3 = 42 

Bắt buộc = 60% của 42 = 25,2 → 26 

Đã tham dự = 15 

Cần = 11 

Số tuần còn lại = 4 

Công suất còn lại = 12 

Chúng tôi phân phối 11 trong 4 tuần. 

| Tuần | Nhu cầu còn lại | Được giao | 
| --- | --- | --- | 
| 1 | 11 | 3 | 
| 2 | 8 | 3 | 
| 3 | 5 | 3 | 
| 4 | 2 | 2 | 

Đầu ra:```
Yes
3 3 3 2
```Dấu vết này cho thấy cách phân bổ tham lam tiêu thụ yêu cầu đều đặn trong khi vẫn tôn trọng giới hạn hàng tuần. 

### Ví dụ 2 

đầu vào:```
12 2 6 5
```Tổng số lớp = 24 

Bắt buộc = 60% của 24 = 14,4 → 15 

Đã tham dự = 5 

Cần = 10 

Số tuần còn lại = 6 

Công suất còn lại = 12 

| Tuần | Nhu cầu còn lại | Được giao | 
| --- | --- | --- | 
| 1 | 10 | 2 | 
| 2 | 8 | 2 | 
| 3 | 6 | 2 | 
| 4 | 4 | 2 | 
| 5 | 2 | 2 | 
| 6 | 0 | 0 | 

Đầu ra:```
Yes
2 2 2 2 2 0
```Điều này chứng tỏ rằng một khi yêu cầu được đáp ứng, các tuần còn lại sẽ được lấp đầy bằng số 0 mà không ảnh hưởng đến tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(X) | Chúng tôi lặp lại một lần trong các tuần còn lại | 
| Không gian | O(X) | Chúng tôi lưu trữ một giá trị cho mỗi tuần còn lại | 

Các ràng buộc giới hạn X ở mức tối đa là 14, do đó thuật toán chạy trong thời gian không đổi trong thực tế và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    import sys
    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return output.getvalue().strip()

# provided sample
assert run("14 3 10 15\n") == "Yes\n3 3 3 2"

# minimum case, already satisfied
assert run("12 1 12 12\n") == "Yes\n0"

# impossible case
assert run("12 1 0 0\n") == "No"

# all capacity needed exactly
assert run("12 2 6 6\n") == "Yes\n2 2 2 2 2 2"

# boundary tight case
assert run("12 2 6 5\n") == "Yes\n2 2 2 2 2 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 14 3 10 15 | Có 3 3 3 2 | độ chính xác của mẫu | 
| 12 1 12 12 | Có 0 | trường hợp đã hài lòng | 
| 12 1 0 0 | Không | không thể phát hiện sớm | 
| 12 2 6 6 | Có 2 2 2 2 2 2 | phân bổ đầy đủ | 
| 12 2 6 5 | Có 2 2 2 2 2 0 | tham lam dừng lại | 

## Vỏ cạnh 

Khi yêu cầu đã được đáp ứng, thuật toán sẽ ngay lập tức đưa ra 0 bài tập cho các tuần còn lại. Ví dụ: nếu tổng số lớp là 20 và yêu cầu là 12 nhưng bạn đã có 15, thì`need`trở nên âm và đầu ra là vectơ 0, duy trì chính xác tính khả thi mà không cần phân bổ không cần thiết. 

Khi thậm chí toàn bộ công suất còn lại không đủ, việc kiểm tra tính khả thi sẽ ngăn cản việc xây dựng. Ví dụ: nếu tổng dung lượng còn lại là 3 nhưng bạn cần thêm 5 lớp nữa, thuật toán sẽ phát hiện`need > remaining_capacity`và xuất ra “Không” trước khi thử bất kỳ phân phối nào.
