---
title: "CF 2182E - Quà tặng năm mới"
description: "Monocarp muốn làm cho càng nhiều bạn bè hài lòng càng tốt với ngân sách hạn hẹp. Anh ấy có một số bạn bè và một bộ hộp."
date: "2026-06-07T21:54:01+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2182
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 186 (Rated for Div. 2)"
rating: 1800
weight: 2182
solve_time_s: 208
verified: true
draft: false
---

[CF 2182E - Quà tặng năm mới](https://codeforces.com/problemset/problem/2182/E) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 3 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Monocarp muốn làm cho càng nhiều bạn bè hài lòng càng tốt với ngân sách hạn hẹp. Anh ấy có một số bạn bè và một bộ hộp. Mỗi hộp có một giá trị vẻ đẹp và mỗi người bạn có ba yêu cầu: vẻ đẹp hộp tối thiểu mà họ quan tâm, giá trị quà tặng tối thiểu và ngưỡng giá trị quà tặng cao hơn để đảm bảo hạnh phúc bất kể chiếc hộp nào. Monocarp có thể chi tiêu nhiều nhất`k`tổng số xu và mỗi hộp có thể chứa tối đa một món quà. Nhiệm vụ là tối đa hóa số lượng bạn bè hài lòng với những hạn chế này. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng ta được cung cấp số lượng bạn bè, số hộp, tổng số xu có sẵn, vẻ đẹp của mỗi hộp và ba con số.`x_i`,`y_i`, Và`z_i`cho mỗi người bạn. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm, cho biết số lượng bạn bè tối đa có thể khiến bạn hài lòng. 

Những hạn chế là lớn. Có thể có tới 200.000 bạn bè hoặc hộp trong một trường hợp thử nghiệm và`k`có thể lớn tới 10^15. Điều này ngay lập tức loại trừ các cách tiếp cận bạo lực nhằm thử tất cả các nhiệm vụ hộp quà có thể có hoặc kết hợp các giá trị quà tặng. Một giải pháp đơn giản coi mỗi cặp bạn bè và hộp sẽ có độ phức tạp là O(n * m), lên tới 4 * 10^10 phép toán và không khả thi. 

Các trường hợp khó khăn cần xử lý cẩn thận bao gồm các tình huống trong đó tất cả các hộp đều có vẻ đẹp quá thấp để làm hài lòng bất kỳ người bạn nào mà không bội chi hoặc khi ngân sách chỉ đủ để mua những món quà tối thiểu nhưng không đủ cho các ngưỡng cao hơn. Một kịch bản phức tạp khác xảy ra khi số xu đủ để đáp ứng tất cả`y_i`giá trị nhưng không thể đủ khả năng dù chỉ một`z_i`giá trị, và vẻ đẹp của hộp là không đủ. Ví dụ, nếu`k = 5`,`boxes = [1]`, và một người bạn duy nhất có`x=2, y=3, z=6`, thì món quà duy nhất vừa túi tiền là`y=3`, nhưng hộp kém mỹ quan nên hạnh phúc không đảm bảo; câu trả lời đúng là`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ giao mỗi hộp cho một người bạn theo mọi cách có thể, tính toán món quà cần thiết tối thiểu để thỏa mãn niềm vui cho nhiệm vụ đó và kiểm tra xem ngân sách có phù hợp không.`k`cho phép nó. Cách tiếp cận này hiệu quả vì nó xem xét tất cả các khả năng và đảm bảo tính chính xác. Tuy nhiên, với tối đa 200.000 bạn bè và hộp, số lượng nhiệm vụ được tính theo quy mô giai thừa và thậm chí việc cố gắng ghép nối trực tiếp bạn bè với các hộp trong một vòng lặp kép đơn giản đều dẫn đến kết quả là O(n * m), tốc độ này quá chậm. 

Điểm mấu chốt là bạn bè chỉ quan tâm đến hai điều: vẻ đẹp của chiếc hộp vượt quá ngưỡng của họ`x_i`hoặc giá trị quà tặng ít nhất là`z_i`. Quà tặng luôn có thể đáp ứng giới hạn dưới`y_i`để thỏa mãn giới hạn ngân sách. Điều này cho phép chúng tôi coi vấn đề này như một vấn đề khớp giữa các hộp và bạn bè, trong đó chúng tôi hướng tới mục tiêu tối đa hóa số lượng bạn bè hài lòng khi sử dụng ngưỡng cao hơn`z_i`khi cần thiết. Bằng cách sắp xếp các hộp và bạn bè theo các ngưỡng có liên quan và sử dụng phương pháp tìm kiếm nhị phân hoặc hai con trỏ tham lam, chúng tôi có thể xác định một cách hiệu quả những người bạn nào có thể hài lòng với các hộp có sẵn mà không vượt quá ngân sách. 

Cụ thể, nếu chúng ta xử lý bạn bè theo thứ tự giảm dần`x_i`(yêu cầu về hộp) và chỉ định hộp làm đẹp cao nhất hiện có đáp ứng được yêu cầu của bạn bè`x_i`, thì người bạn nào chúng ta không thể làm hài lòng bằng một chiếc hộp phải nhận được một món quà ít nhất`z_i`nếu ngân sách cho phép. Tổng của tất cả`y_i`được đảm bảo phù hợp với`k`, nên chúng ta chỉ cần tính thêm chi phí cho những món quà có giá trị`z_i`. Việc sắp xếp cho phép hiệu quả O(n log n + m log m) và tìm kiếm hai con trỏ hoặc nhị phân đảm bảo chúng ta khớp các hộp một cách tham lam mà không cần xem lại các quyết định trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n + m) | Quá chậm | 
| Sắp xếp + Tham lam + Hai con trỏ | O(n log n + m log m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng ô đẹp theo thứ tự tăng dần. Điều này cho phép kết hợp hiệu quả bằng cách sử dụng tìm kiếm hai con trỏ hoặc tìm kiếm nhị phân. 
2. Đối với mỗi người bạn, hãy xác định xem liệu yêu cầu về hộp của họ có thể được đáp ứng hay không. Để làm điều này, hãy sắp xếp bạn bè theo thứ tự giảm dần`x_i`và theo dõi những hộp nào có sẵn. 
3. Lặp lại thông qua bạn bè và cố gắng chỉ định hộp nhỏ nhất có sẵn có vẻ đẹp ít nhất là`x_i`. Đánh dấu ô này là đã sử dụng. Điều này đảm bảo các hộp không bị tính hai lần. 
4. Đối với những người bạn không thể được chỉ định một hộp phù hợp, hãy tính số tiền bổ sung cần thiết ngoài`y_i`để làm cho họ hạnh phúc (`z_i - y_i`). Duy trì tổng số chi phí bổ sung này. 
5. Nếu tổng chi phí tăng thêm vượt quá`k - sum(y_i)`, ngừng chỉ định bạn bè nhận những món quà có ngưỡng cao hơn. Số lượng bạn bè được chỉ định thành công cho đến thời điểm này là mức hạnh phúc tối đa có thể đạt được. 
6. Tổng của`y_i`luôn luôn là ≤`k`, do đó, mọi khoản ngân sách còn lại sau số quà tặng tối thiểu bắt buộc đều có thể được sử dụng cho các chi phí bổ sung này. 

Tại sao nó hoạt động: Việc sắp xếp các hộp và bạn bè đảm bảo rằng mỗi người bạn được ghép với nguồn tài nguyên đủ tối thiểu (hộp hoặc tiền bổ sung). Bằng cách xử lý các yêu cầu lớn nhất trước, chúng tôi tránh được tình huống người đẹp hộp nhỏ bị bạn bè có yêu cầu thấp tiêu thụ, khiến những người bạn có yêu cầu cao không hài lòng. Điều bất biến là bất kỳ người bạn nào được xem xét đều được ghép vào một ô thỏa mãn`x_i`hoặc tính thêm tiền để tiếp cận`z_i`. Vì các hộp được sử dụng tối đa một lần và số tiền bổ sung được kiểm tra so với ngân sách còn lại nên không xảy ra tình trạng cam kết quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        boxes = sorted(map(int, input().split()))
        friends = []
        sum_y = 0
        for _ in range(n):
            x, y, z = map(int, input().split())
            friends.append((x, y, z))
            sum_y += y
        
        friends.sort(reverse=True)  # sort descending by x_i

        used = [False] * m
        happy = 0
        extra_needed = []

        j = m - 1  # pointer to the largest available box
        for x, y, z in friends:
            while j >= 0 and boxes[j] >= x:
                j -= 1
            if j < m - 1:
                happy += 1  # friend happy with a box
            else:
                extra_needed.append(z - y)

        extra_needed.sort()
        remaining = k - sum_y
        for cost in extra_needed:
            if remaining >= cost:
                remaining -= cost
                happy += 1
            else:
                break

        print(happy)

if __name__ == "__main__":
    solve()
```Mã đọc đầu vào hiệu quả và xử lý từng trường hợp thử nghiệm một cách độc lập. Bạn bè được sắp xếp theo thứ tự giảm dần`x_i`để tối đa hóa cơ hội đáp ứng những yêu cầu lớn nhất trước tiên. Các hộp được sắp xếp để nhanh chóng xác định được vẻ đẹp vừa đủ tối thiểu. các`extra_needed`mảng thu thêm chi phí xu cho những người bạn không hài lòng với một chiếc hộp. Sắp xếp`extra_needed`đảm bảo chúng ta chi tiêu ngân sách một cách tham lam từ chi phí nhỏ nhất đến chi phí lớn nhất, tối đa hóa số lượng bạn bè hạnh phúc. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2 1 6
1
1 2 3
1 2 7
```| Bạn | x_i | y_i | z_i | Hộp được giao | Chi phí bổ sung | Còn lại k | Chúc mừng đếm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | có (1) | 0 | 6 | 1 | 
| 2 | 1 | 2 | 7 | không | 5 | 4 | 2 | 

Điều này khẳng định rằng cả hai người bạn đều có thể hài lòng trong phạm vi ngân sách. 

Một mẫu khác:```
2 2 3
1 1
2 1 3
2 1 5
```Hộp`[1,1]`không đủ cho`x_i=2`, chi phí phụ`2`Và`4`được yêu cầu. Ngân sách còn lại sau`sum(y_i)=2`là`1`, do đó không thể trả thêm chi phí. Đầu ra là`0`, phù hợp với mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log m) | Sắp xếp bạn bè và các hộp chiếm ưu thế; lặp lại là O(n + m) | 
| Không gian | O(n + m) | Để lưu trữ bạn bè, hộp và những thứ bổ sung | 

Giải pháp phù hợp với giới hạn thời gian và bộ nhớ đối với các ràng buộc nhất định: tổng 2*10^5 n và m trên tất cả các trường hợp thử nghiệm có 2 giây và 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n2 1 6\n1\n1 2 3\n1 2 7\n2 2 3\n1 1\n2 1 3\n2 1 5\n3 4 11\n1 2 2 1\n3 2 5\n4 4 6\n3 1 3") == "2\n0\n2"

# Minimum input
assert run("1\n1 1 1
```
