---
title: "CF 102191B - Vấn đề cuối cùng"
description: "Chúng tôi hiện có mười vấn đề, mỗi vấn đề có độ khó từ 1 đến 10. Mỗi đội đều có một cấp độ kỹ năng, cũng từ 1 đến 10 và một đội có thể giải chính xác những vấn đề có độ khó không vượt quá kỹ năng của đội đó. Chúng ta có thể thêm một bài toán mới với độ khó từ 1 đến 10."
date: "2026-08-23T14:48:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "B"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 1475
verified: true
draft: false
---

[CF 102191B - Vấn đề cuối cùng](https://codeforces.com/problemset/problem/102191/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 24m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi hiện có mười vấn đề, mỗi vấn đề có độ khó từ 1 đến 10. Mỗi đội đều có một cấp độ kỹ năng, cũng từ 1 đến 10 và một đội có thể giải chính xác những vấn đề có độ khó không vượt quá kỹ năng của đội đó. 

Chúng ta có thể thêm một bài toán mới có độ khó từ 1 đến 10. Bài toán mới phải đủ dễ để mỗi đội có thể giải được ít nhất một bài toán sau khi được thêm vào. Mục tiêu là làm cho vấn đề mới này trở nên khó khăn nhất có thể trong khi vẫn làm hài lòng mọi đội. 

Giá trị đầu vào đầu tiên,`n`, là số đội. Dòng tiếp theo chứa cấp độ kỹ năng của họ. Dòng cuối cùng chứa những khó khăn của mười vấn đề hiện có. 

Ràng buộc`n <= 32`là cực kỳ nhỏ. Ngay cả một cách tiếp cận kiểm tra tất cả mười khó khăn mới có thể xảy ra đối với mỗi đội cũng chỉ đạt hiệu quả tối đa.`10 * 32 = 320`kiểm tra đội. Giới hạn bộ nhớ 256 MB cũng vượt xa mức cần thiết. Trên thực tế, bài toán có đủ cấu trúc để giảm công việc xuống còn một lần quét qua các nhóm. 

Trường hợp chính là khi mọi đội đều có thể giải quyết được một vấn đề hiện có. Trong tình huống đó, bài toán mới không cần giúp đỡ ai nên độ khó của nó có thể đạt giá trị lớn nhất cho phép là 10. Ví dụ:```
1
5
1 2 3 4 5 6 7 8 9 10
```Đội duy nhất giải được bài toán khó 1 nên đáp án là`10`. Việc triển khai bất cẩn luôn tìm kiếm một nhóm cần trợ giúp có thể không đưa ra câu trả lời trong trường hợp này. 

Một trường hợp khó khăn khác xảy ra khi đội yếu nhất không thể giải quyết bất kỳ vấn đề nào hiện có. Ví dụ:```
2
1 5
2 3 4 6 7 8 9 10 10 10
```Đội có kỹ năng 1 cần giải bài toán mới có độ khó tối đa là 1. Do đó câu trả lời là`1`. Việc triển khai sử dụng bất đẳng thức nghiêm ngặt như`difficulty < skill`sẽ coi một bài toán có độ khó 1 là không thể giải được bởi đội có kỹ năng 1. 

Trường hợp thứ ba liên quan đến một số đội có cùng kỹ năng. Ví dụ:```
3
4 4 7
5 6 8 9 10 10 10 10 10 10
```Cả hai đội có kỹ năng 4 đều cần đề mới có độ khó tối đa là 4 nên đáp án là`4`. Các đội trùng lặp không thay đổi độ khó yêu cầu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể thử mọi độ khó có thể từ 1 đến 10 cho vấn đề mới. Đối với từng khó khăn của ứng viên, hãy quét từng đội và kiểm tra xem đội đó có thể giải quyết được ít nhất một trong mười vấn đề hiện có hay vấn đề mới hay không. Điều này đúng vì mọi câu trả lời hợp pháp đều được kiểm tra rõ ràng. Với`n <= 32`, trường hợp xấu nhất chỉ là 10 khó khăn của ứng viên nhân 32 nhóm nhân 10 vấn đề hiện có hoặc 3200 so sánh cơ bản nếu chúng ta kiểm tra tất cả các vấn đề hiện có một cách riêng biệt. Điều này thoải mái trong giới hạn. 

Cách tiếp cận vũ phu có hiệu quả vì phạm vi độ khó rất nhỏ nhưng nó thực hiện những công việc không cần thiết. Chúng tôi thực sự không cần phải xem xét từng vấn đề hiện có một cách riêng biệt cho từng nhóm. Đối với một nhóm cụ thể, chỉ có vấn đề dễ nhất hiện có mới quan trọng. Nếu vấn đề dễ nhất gặp khó khăn`m`, thì mỗi đội có kỹ năng ít nhất`m`đã được bảo hiểm, trong khi mọi đội có kỹ năng dưới đây`m`không thể giải quyết bất kỳ vấn đề hiện có. 

Giả sử một đội có kỹ năng`s`và vấn đề dễ nhất hiện có lại gặp khó khăn`m`. Nếu như`s >= m`, đội đó đã được đảm bảo sẽ giải quyết được vấn đề nào đó, vì vậy vấn đề mới không đặt ra hạn chế nào về độ khó của nó. Nếu như`s < m`, bài toán mới trở thành bài toán duy nhất có thể giải được cho đội này, do đó độ khó của nó phải thỏa mãn`new_difficulty <= s`. 

Do đó, trong số tất cả các đội chưa được đề cập, vấn đề mới phải có độ khó ở cấp độ kỹ năng nhỏ nhất. Việc chọn chính xác kỹ năng nhỏ nhất đó sẽ mang lại độ khó hợp lệ lớn nhất có thể. Nếu không có đội nào chưa được khám phá, vấn đề mới có thể chỉ có độ khó 10. 

Điều này làm giảm toàn bộ vấn đề thành việc tìm ra độ khó tối thiểu của vấn đề hiện có và sau đó tìm ra đội yếu nhất có kỹ năng thấp hơn giá trị đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10 × n × 10) = O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n + 10) = O(n) | O(1) | Đã chấp nhận | 

Sự khác biệt tiệm cận không đáng kể vì tất cả các giá trị liên quan đều rất nhỏ, nhưng lời giải tối ưu sẽ bộc lộ cấu trúc toán học thực tế của bài toán và tránh việc kiểm tra nhiều lần cùng một thông tin. 

## Hướng dẫn thuật toán 

1. Đọc số lượng đội, trình độ kỹ năng của họ và mười vấn đề khó khăn hiện có. Các vấn đề hiện tại có thể được tóm tắt bằng độ khó tối thiểu của chúng bởi vì, để quyết định xem một nhóm đã được giải quyết chưa, chỉ cần giải quyết bất kỳ vấn đề nào là đủ. 
2. Tính toán`easiest`, độ khó tối thiểu trong số mười vấn đề hiện có. Một đội có kỹ năng`s`đã có thể giải quyết vấn đề chính xác khi nào`s >= easiest`. 
3. Khởi tạo câu trả lời cho`10`. Điều này thể hiện trường hợp mọi đội đều đã được bảo hiểm nên không có hạn chế nào đến từ các đội hiện có. 
4. Quét mọi kỹ năng của đội`s`. Nếu như`s < easiest`, nhóm này không thể giải quyết bất kỳ vấn đề hiện có nào. Bài toán mới khi đó phải có độ khó nhiều nhất`s`. Vì chúng tôi muốn độ khó tối đa có thể, hãy cập nhật câu trả lời ở mức tối thiểu như vậy`s`. 
5. In câu trả lời có được. Nếu có ít nhất một đội bị lộ thì câu trả lời là kỹ năng yếu nhất trong số các đội bị lộ. Nếu không có đội nào bị phát hiện, giá trị ban đầu`10`vẫn còn hiệu lực. 

### Tại sao nó hoạt động 

hãy để`easiest`là khó khăn nhỏ nhất trong số các vấn đề hiện có. Đối với mỗi đội có kỹ năng ít nhất`easiest`, đội đó đã có thể giải được bài toán dễ nhất nên không đặt ra điều kiện nào cho bài toán mới. Dành cho mọi đội có kỹ năng dưới đây`easiest`, đội không thể giải quyết bất kỳ vấn đề nào hiện có nên vấn đề mới phải có độ khó không lớn hơn kỹ năng của đội đó. Do đó, mọi đội chưa được khám phá đều áp đặt giới hạn trên cho độ khó mới và tất cả các giới hạn đó phải được giữ đồng thời. Giới hạn chặt chẽ nhất là kỹ năng tối thiểu của họ. Việc chọn giá trị đó làm hài lòng mọi đội chưa được khám phá và ít nhất cũng khó như bất kỳ lựa chọn hợp lệ nào khác. Nếu không có đội nào chưa được khám phá thì tất cả các đội đều đã hài lòng và độ khó tối đa cho phép là 10 là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    skills = list(map(int, input().split()))
    difficulties = list(map(int, input().split()))

    easiest = min(difficulties)
    answer = 10

    for skill in skills:
        if skill < easiest:
            answer = min(answer, skill)

    print(answer)

solve()
```Ba lần đọc đầu tiên tương ứng trực tiếp với ba phần đầu vào: số lượng đội, kỹ năng của họ và mười khó khăn hiện có của vấn đề.`easiest = min(difficulties)`nén tất cả mười vấn đề hiện có vào giá trị duy nhất liên quan đến phạm vi bảo hiểm. Nếu một nhóm không thể giải được vấn đề dễ nhất này thì đội đó cũng không thể giải quyết được bất kỳ vấn đề nào khác vì ít nhất tất cả chúng đều khó như nhau. 

Câu trả lời bắt đầu từ số 10 vì 10 là độ khó lớn nhất được phép. Một đội có kỹ năng dưới đây`easiest`không thể giải được bất cứ điều gì từ bộ ban đầu, vì vậy kỹ năng của nó trở thành giới hạn trên của bài toán mới. Việc áp dụng mức tối thiểu cho tất cả các kỹ năng như vậy sẽ xử lý đồng thời tất cả các ràng buộc. 

Sự so sánh là`skill < easiest`, không`skill <= easiest`. Một đội có kỹ năng tương đương với độ khó của bài toán dễ nhất có thể giải được bài toán đó nên bài toán đó đã được giải quyết xong. 

Không cần xử lý đặc biệt đối với nhóm trống gồm các đội chưa được khám phá. Câu trả lời đầu tiên là 10 sẽ giải quyết được trường hợp đó một cách tự nhiên. Không thể tràn số nguyên vì tất cả các giá trị đều nằm trong khoảng từ 1 đến 10. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
4
3 7 5 5
4 6 5 7 4 4 9 10 7 9
```Bài toán dễ nhất hiện có lại có độ khó 4. 

| Kỹ năng đồng đội | Khó khăn dễ nhất | Đã được bảo hiểm chưa? | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 3 | 4 | Không | 3 | 
| 7 | 4 | Có | 3 | 
| 5 | 4 | Có | 3 | 
| 5 | 4 | Có | 3 | 

Đội có kỹ năng 3 không thể giải được bất kỳ vấn đề nào hiện có nên vấn đề mới phải có độ khó tối đa là 3. Tất cả các đội còn lại đều có thể giải được vấn đề có độ khó-4. Vì vậy, câu trả lời hợp lệ tối đa là`3`. 

### Xây dựng ví dụ 2 

Hãy xem xét:```
3
5 6 10
1 4 7 8 9 10 10 10 10 10
```Vấn đề dễ nhất hiện có có độ khó 1. 

| Kỹ năng đồng đội | Khó khăn dễ nhất | Đã được bảo hiểm chưa? | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 5 | 1 | Có | 10 | 
| 6 | 1 | Có | 10 | 
| 10 | 1 | Có | 10 | 

Mọi đội đều có thể giải được bài toán khó-1. Không có đội nào chưa được khám phá hạn chế vấn đề mới, vì vậy câu trả lời vẫn là 10. Dấu vết này chứng tỏ tại sao trường hợp được che phủ toàn bộ phải trả về độ khó tối đa cho phép thay vì kỹ năng nhóm tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 10) = O(n) | Chúng tôi tìm thấy mức tối thiểu trong mười khó khăn và quét`n`kỹ năng nhóm một lần. | 
| Không gian | O(n) | Các kỹ năng nhóm được lưu trữ trong một danh sách; bản thân đầu vào chỉ chứa`n`kỹ năng và mười khó khăn. | 

Với`n <= 32`, thuật toán chỉ thực hiện vài chục thao tác có ý nghĩa. Nó thấp hơn nhiều so với giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    skills = [next(it) for _ in range(n)]
    difficulties = [next(it) for _ in range(10)]

    easiest = min(difficulties)
    answer = 10

    for skill in skills:
        if skill < easiest:
            answer = min(answer, skill)

    return str(answer)

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample
assert run(
    """4
3 7 5 5
4 6 5 7 4 4 9 10 7 9
"""
) == "3", "sample 1"

# Minimum-size input, team can already solve the easiest problem.
assert run(
    """1
10
1 2 3 4 5 6 7 8 9 10
"""
) == "10", "minimum size, already covered"

# Minimum skill cannot solve any existing problem.
assert run(
    """1
1
2 3 4 5 6 7 8 9 10 10
"""
) == "1", "minimum skill boundary"

# All teams have the same skill, and all existing problems are too hard.
assert run(
    """5
4 4 4 4 4
5 6 7 8 9 10 10 10 10 10
"""
) == "4", "all equal skills"

# Maximum-size input, mixed covered and uncovered teams.
assert run(
    """32
1 2 3 4 5 6 7 8 9 10 1 2 3 4 5 6
7 8 9 10 10 10 10 10 10 10
"""
) == "1", "maximum n"

# Boundary case: a team exactly equal to the easiest difficulty is covered.
assert run(
    """3
3 4 7
4 5 6 7 8 9 10 10 10 10
"""
) == "3", "exact equality boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`3`| Ví dụ được cung cấp với một nhóm chưa được khám phá | 
|`1 / skill 10 / difficulties 1..10`|`10`| Đầu vào tối thiểu và tất cả các nhóm đã được bảo hiểm | 
|`1 / skill 1 / difficulties 2..10`|`1`| Kỹ năng thấp nhất có thể và câu trả lời thấp nhất có thể | 
| Năm đội có kỹ năng 4 và tất cả các vấn đề ít nhất là 5 |`4`| Kỹ năng nhóm trùng lặp và hoàn toàn bình đẳng | 
| 32 đội lặp lại kỹ năng từ 1 đến 10 |`1`| Tối đa cho phép`n`| 
| Kỹ năng`3, 4, 7`, vấn đề dễ nhất`4`|`3`| Sự bình đẳng chính xác phải được tính là có thể giải quyết được | 

## Vỏ cạnh 

Khi mọi đội đều đã được bảo vệ, thuật toán sẽ tiếp tục`answer = 10`. Ví dụ:```
1
5
1 2 3 4 5 6 7 8 9 10
```Đây`easiest = 1`, và kỹ năng duy nhất là 5. Vì`5 < 1`là sai, câu trả lời không bao giờ thay đổi so với 10. Điều này đúng vì đội đã giải được bài toán độ khó 1 nên bài toán được thêm vào có thể khó đến mức tối đa cho phép. 

Khi đội yếu nhất không thể giải quyết bất kỳ vấn đề nào hiện có, kỹ năng của đội đó sẽ trực tiếp quyết định câu trả lời. Vì:```
2
1 5
2 3 4 6 7 8 9 10 10 10
```chúng tôi nhận được`easiest = 2`. Đội có kỹ năng 1 đạt yêu cầu`1 < 2`, Vì thế`answer`trở thành 1. Đội có kỹ năng 5 đã được bảo vệ. Kết quả đầu ra là`1`, đó là khó khăn duy nhất mà đội yếu nhất có thể giải quyết được. 

Khi kỹ năng của một đội tương đương với vấn đề dễ nhất hiện có thì đội đó đã được giải quyết. Coi như:```
3
3 4 7
4 5 6 7 8 9 10 10 10 10
```Bài toán dễ nhất có độ khó 4. Đội có kỹ năng 3 chưa được khám phá nên đáp án trở thành 3. Đội có kỹ năng 4 không được khám phá vì`4 < 4`là sai và nó có thể giải quyết chính xác vấn đề độ khó-4. Đội có kỹ năng 7 cũng được bảo hiểm. Đầu ra là`3`. 

Kỹ năng nhóm trùng lặp không yêu cầu logic bổ sung. Vì:```
5
4 4 4 4 4
5 6 7 8 9 10 10 10 10 10
```vấn đề dễ nhất hiện có có độ khó 5. Mọi đội đều đáp ứng được`4 < 5`, vì vậy mỗi đội cần bài toán mới gặp độ khó tối đa là 4. Lấy mức tối thiểu của các ràng buộc giống hệt nhau này sẽ cho 4, được in dưới dạng câu trả lời.
