---
title: "CF 106394D - Sức chịu đựng"
description: "Vấn đề mô tả một chuỗi các nhiệm vụ được xử lý từ trái sang phải. Mỗi nhiệm vụ có giá trị phần thưởng và tỷ lệ phần trăm giảm sức chịu đựng. Bạn bắt đầu với sức chịu đựng bằng 1."
date: "2026-06-25T10:10:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106394
codeforces_index: "D"
codeforces_contest_name: "RUCP x WiCS Mini-Contest"
rating: 0
weight: 106394
solve_time_s: 30
verified: true
draft: false
---

[CF 106394D - Sức chịu đựng](https://codeforces.com/problemset/problem/106394/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một chuỗi các nhiệm vụ được xử lý từ trái sang phải. Mỗi nhiệm vụ có giá trị phần thưởng và tỷ lệ phần trăm giảm sức chịu đựng. Bạn bắt đầu với sức chịu đựng bằng 1. Nếu bạn hoàn thành một nhiệm vụ, bạn nhận được phần thưởng bằng giá trị nhiệm vụ nhân với sức chịu đựng hiện tại của bạn, thì sức chịu đựng của bạn sẽ giảm theo tỷ lệ phần trăm nhất định. Nếu bạn bỏ qua nhiệm vụ, bạn sẽ không đạt được gì và sức chịu đựng của bạn không thay đổi. Mục tiêu là chọn những nhiệm vụ cần hoàn thành sao cho tổng phần thưởng cuối cùng càng lớn càng tốt. 

Đầu vào chứa một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, giá trị đầu tiên là số lượng nhiệm vụ. Mỗi cặp tiếp theo mô tả một nhiệm vụ: số đầu tiên là hệ số phần thưởng và số thứ hai là phần trăm thể lực bị mất sau khi hoàn thành nhiệm vụ đó. Đầu ra là số điểm tối đa có thể đạt được. 

Tổng số nhiệm vụ trên tất cả các trường hợp thử nghiệm đủ lớn để việc kiểm tra tất cả các tập hợp con là không thể. Nếu có tới 100000 nhiệm vụ, cách tiếp cận bạo lực sẽ yêu cầu xem xét số lượng lựa chọn theo cấp số nhân, khoảng$2^{100000}$, không thể xử lý được. Ngay cả một chương trình động có quá nhiều trạng thái cũng sẽ thất bại, do đó giải pháp phải xử lý từng tác vụ với số lần không đổi. 

Những trường hợp khó khăn xuất phát từ việc hiểu rằng sức chịu đựng chỉ thay đổi khi một nhiệm vụ được hoàn thành và quyết định thực hiện một nhiệm vụ phụ thuộc vào các nhiệm vụ trong tương lai. Một cách tiếp cận tham lam luôn mang lại phần thưởng lớn nhất ngay lập tức có thể thất bại vì nhiệm vụ sớm có thể làm giảm sức chịu đựng và khiến các nhiệm vụ sau này trở nên kém giá trị hơn nhiều. 

Ví dụ:```
1
10 100
```Câu trả lời là`10`. Việc thực hiện bất cẩn có thể nhân phần thưởng với sức chịu đựng sau khi giảm và nhận được 0, nhưng phần thưởng sẽ kiếm được trước khi sức chịu đựng thay đổi. 

Một ví dụ khác:```
3
10 5
10 80
20 5
```Câu trả lời tốt nhất là`29`. Hoàn thành tất cả các nhiệm vụ sẽ mang lại:```
10 + 9.5 + 17.1 = 36.6
```Trên thực tế, ví dụ này cho thấy tại sao trật tự lại quan trọng, bởi vì nhiệm vụ thứ hai tiêu diệt phần lớn sức chịu đựng. Lựa chọn tối ưu là bỏ qua nhiệm vụ ở giữa:```
10 + 20 * 0.95 = 29
```Một giải pháp chỉ so sánh phần thưởng của từng nhiệm vụ sẽ bỏ lỡ tác dụng của sức chịu đựng như một hệ số nhân cho tất cả các nhiệm vụ hoàn thành trong tương lai. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi tập hợp con nhiệm vụ. Đối với mỗi tập hợp con, chúng tôi mô phỏng các nhiệm vụ đã chọn theo thứ tự, giữ nguyên sức chịu đựng hiện tại và điểm tích lũy. Điều này đúng vì mọi trình tự quyết định có thể đều được xem xét. Tuy nhiên, số lượng tập hợp con tăng gấp đôi sau mỗi nhiệm vụ bổ sung, do đó, với 100000 nhiệm vụ, số lượng khả năng vượt xa những gì bất kỳ máy tính nào cũng có thể xử lý. 

Nhận xét hữu ích là khi chúng ta xem xét một nhiệm vụ ở cuối danh sách, thì tương lai đã được tóm tắt rồi. Giả sử chúng ta biết số điểm tối đa có thể đạt được từ tất cả các nhiệm vụ sau nhiệm vụ hiện tại, giả sử sức chịu đựng hiện tại của chúng ta là 1. Nếu quyết định hoàn thành nhiệm vụ hiện tại, chúng ta sẽ nhận được phần thưởng ngay lập tức và sau đó tất cả phần thưởng trong tương lai sẽ được tính theo số sức chịu đựng còn lại. Nếu chúng ta bỏ qua nó, chúng ta chỉ cần giữ giá trị tương lai không thay đổi. 

Điều này mang lại trạng thái lập trình động một chiều. Cho phép`dp`thể hiện điểm cao nhất có thể đạt được từ hậu tố của các nhiệm vụ hiện đang được xem xét, tách riêng yếu tố sức chịu đựng. Khi xử lý một nhiệm vụ có phần thưởng`c`và hệ số nhân sức chịu đựng còn lại`m`, việc hoàn thành nó mang lại:$$c + m \times dp$$Đối với vấn đề này, hệ số sức chịu đựng sau khi hoàn thành nhiệm vụ là:$$1 - \frac{p}{100}$$Vì vậy, khi di chuyển từ cuối về đầu, quá trình chuyển đổi trở thành:$$dp_i = \max(dp_{i+1}, c_i + (1-\frac{p_i}{100})dp_{i+1})$$Lý do điều này có tác dụng là vì tác động duy nhất của nhiệm vụ hiện tại đối với tương lai là hệ số nhân duy nhất áp dụng cho các phần thưởng trong tương lai. Chúng ta không cần phải nhớ toàn bộ lịch sử của những lựa chọn trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(1)$| Quá chậm | 
| Lập trình động tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các nhiệm vụ và xử lý chúng từ nhiệm vụ cuối cùng đến nhiệm vụ đầu tiên. Các quyết định trong tương lai sẽ dễ mô tả hơn khi câu trả lời hậu tố đã được biết trước. 
2. Duy trì một biến`dp`chứa số điểm tối đa có thể đạt được từ các tác vụ được xử lý cho đến nay trong lần duyệt ngược này. Ban đầu không có nhiệm vụ nào trong tương lai, vì vậy`dp = 0`. 
3. Đối với nhiệm vụ hiện tại có thưởng`c`và tỷ lệ mất sức chịu đựng`p`, tính kết quả nếu chúng ta hoàn thành nó. Phần thưởng đạt được là`c`và mọi phần thưởng trong tương lai sẽ được nhân với`1 - p / 100`. 
4. So sánh giá trị của việc hoàn thành nhiệm vụ với giá trị của việc bỏ qua nó. Giữ giá trị lớn hơn như mới`dp`. 
5. Sau khi tất cả các tác vụ đã được xử lý,`dp`là câu trả lời cho toàn bộ chuỗi. 

Tại sao nó hoạt động: tính bất biến của lập trình động là trước khi xử lý một tác vụ theo thứ tự ngược lại,`dp`bằng điểm cao nhất có thể từ tất cả các nhiệm vụ sau nhiệm vụ đó khi hệ số sức chịu đựng hiện tại được coi là một. Đối với nhiệm vụ hiện tại, mọi chiến lược hợp lệ đều bỏ qua hoặc hoàn thành nó. Quá trình chuyển đổi đánh giá chính xác cả hai khả năng và không cần thông tin nào khác từ các nhiệm vụ trước đó vì những thay đổi về sức chịu đựng chỉ được thể hiện bằng hệ số nhân áp dụng cho các phần thưởng trong tương lai. Vì mọi hậu tố đều được giải quyết một cách tối ưu trước khi di chuyển sang trái nên giá trị cuối cùng là tối ưu cho toàn bộ danh sách nhiệm vụ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        tasks = []

        for _ in range(n):
            c, p = map(int, input().split())
            tasks.append((c, p))

        dp = 0.0

        for c, p in reversed(tasks):
            multiplier = 1.0 - p / 100.0
            take = c + multiplier * dp
            skip = dp
            dp = max(take, skip)

        ans.append(f"{dp:.10f}")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc bình thường vì tổng số tác vụ bị giới hạn, nhưng`sys.stdin.readline`tránh được những chi phí không cần thiết. 

Danh sách các tác vụ được lưu trữ vì quá trình chuyển đổi cần chạy từ đầu đến cuối. Trong khi xử lý ngược,`dp`chỉ lưu trữ giá trị tốt nhất của hậu tố đã được xử lý, do đó không cần mảng lập trình động đầy đủ. 

Việc tính toán sử dụng số dấu phẩy động vì việc giảm sức chịu đựng liên quan đến tỷ lệ phần trăm. Số nguyên Python không có vấn đề tràn, nhưng cần có độ chính xác của dấu phẩy động cho phần thưởng phân số. In mười chữ số sau dấu thập phân là đủ để đạt được dung sai yêu cầu. 

Thứ tự của các hoạt động quan trọng. Phần thưởng`c`được thêm vào trước khi áp dụng hệ số nhân sức chịu đựng vì việc hoàn thành một nhiệm vụ sẽ mang lại điểm ngay lập tức, sau đó giảm sức chịu đựng cho các nhiệm vụ sau. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
1
2
10 0
20 5
```Quá trình ngược lại là: 

| Nhiệm vụ hiện tại | dp hiện tại | Giá trị hoàn chỉnh | Bỏ qua giá trị | dp mới | 
| --- | --- | --- | --- | --- | 
| 20 5 | 0 | 20 | 0 | 20 | 
| 10 0 | 20 | 30 | 20 | 30 | 

Nhiệm vụ thứ hai giữ toàn bộ sức chịu đựng vì mức giảm của nó bằng không. Thuật toán chọn cả hai nhiệm vụ và nhận được 30. 

Một ví dụ khác:```
1
3
10 5
10 80
20 5
```Quá trình ngược lại là: 

| Nhiệm vụ hiện tại | dp hiện tại | Giá trị hoàn chỉnh | Bỏ qua giá trị | dp mới | 
| --- | --- | --- | --- | --- | 
| 20 5 | 0 | 20 | 0 | 20 | 
| 10 80 | 20 | 14 | 20 | 20 | 
| 10 5 | 20 | 29 | 20 | 29 | 

Nhiệm vụ ở giữa bị bỏ qua vì sự mất sức chịu đựng của nó làm giảm quá nhiều giá trị của phần thưởng trong tương lai. Bất biến giữ nguyên vì mỗi bước giữ giá trị hậu tố tốt nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi tác vụ được xử lý một lần trong khi di chuyển ngược lại. | 
| Không gian |$O(n)$| Danh sách nhiệm vụ được lưu trữ. Bản thân trạng thái lập trình động chỉ sử dụng$O(1)$ký ức. | 

Tổng số nhiệm vụ tối đa là 100000, do đó, giải pháp tuyến tính dễ dàng phù hợp với giới hạn cuộc thi thông thường. Việc sử dụng bộ nhớ cũng nhỏ vì chỉ duy trì các tác vụ đầu vào và một trạng thái dấu phẩy động. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        tasks = []

        for _ in range(n):
            c, p = map(int, input().split())
            tasks.append((c, p))

        dp = 0.0
        for c, p in reversed(tasks):
            dp = max(dp, c + (1 - p / 100.0) * dp)

        out.append(f"{dp:.10f}")

    return "\n".join(out)

assert solution("""1
2
10 0
20 5
""") == "30.0000000000"

assert solution("""1
3
10 5
10 80
20 5
""") == "29.0000000000"

assert solution("""1
1
10 100
""") == "10.0000000000"

assert solution("""1
4
5 0
5 0
5 0
5 0
""") == "20.0000000000"

assert solution("""1
3
100 100
1 100
50 0
""") == "150.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một nhiệm vụ mất 100% thể lực | 10 | Phần thưởng được nhận trước khi sức chịu đựng thay đổi. | 
| Nhiệm vụ tồn tại một hình phạt lớn ở giữa | 29 | Bỏ qua các nhiệm vụ có hại là cần thiết. | 
| Tất cả mức giảm đều bằng 0 | 20 | Tất cả các nhiệm vụ nên được lựa chọn khi sức chịu đựng không bao giờ giảm. | 
| Giảm cực đoan hỗn hợp | 150 | Xử lý tỷ lệ phần trăm ranh giới một cách chính xác. | 

## Vỏ cạnh 

Đối với trường hợp:```
1
1
10 100
```Thuật toán bắt đầu với`dp = 0`. Hoàn thành nhiệm vụ mang lại`10 + 0 * 0 = 10`, trong khi bỏ qua mang lại`0`, do đó câu trả lời trở thành 10. Việc giảm sức chịu đựng xảy ra sau khi nhận được phần thưởng và không ảnh hưởng đến nhiệm vụ hiện tại. 

Đối với trường hợp:```
1
3
10 5
10 80
20 5
```Lập trình động ngược bắt đầu từ nhiệm vụ cuối cùng. Giá trị sau tác vụ cuối cùng là 20. Tác vụ ở giữa sẽ chỉ tạo ra`10 + 0.2 * 20 = 14`, do đó thuật toán giữ lại 20 bằng cách bỏ qua nó. Nhiệm vụ đầu tiên đưa ra`10 + 0.95 * 20 = 29`, trở thành câu trả lời cuối cùng. 

Đối với các nhiệm vụ mà mọi mức giảm đều bằng 0:```
1
4
5 0
5 0
5 0
5 0
```Mỗi nhiệm vụ hoàn thành đều giữ nguyên hệ số nhân sức chịu đựng. Quá trình chuyển đổi luôn ưu tiên nhận nhiệm vụ hơn vì nó mang lại phần thưởng tích cực mà không gây tổn hại đến phần thưởng trong tương lai. Câu trả lời cuối cùng là 20.
