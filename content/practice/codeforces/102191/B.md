---
title: "CF 102191B - Vấn đề cuối cùng"
description: "Chúng tôi có một cuộc thi với đúng mười vấn đề hiện có. Mỗi đội có trình độ kỹ năng từ 1 đến 10 và mọi vấn đề đều có độ khó từ 1 đến 10. Một đội có thể giải quyết vấn đề một cách chính xác khi độ khó của vấn đề không lớn hơn kỹ năng của đội."
date: "2026-08-18T19:34:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "B"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 180
verified: false
draft: false
---

[CF 102191B - Vấn đề cuối cùng](https://codeforces.com/problemset/problem/102191/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cuộc thi với đúng mười vấn đề hiện có. Mỗi đội có trình độ kỹ năng từ 1 đến 10 và mọi vấn đề đều có độ khó từ 1 đến 10. Một đội có thể giải quyết vấn đề một cách chính xác khi độ khó của vấn đề không lớn hơn kỹ năng của đội. 

Chúng tôi có thể thêm một vấn đề mới. Độ khó của nó cũng phải nằm trong khoảng từ 1 đến 10. Bài toán mới phải đảm bảo rằng mỗi đội đều có ít nhất một bài toán có thể giải được, tính cả mười bài toán ban đầu và bài toán mới. Mục tiêu là làm cho vấn đề mới trở nên khó khăn nhất có thể trong khi vẫn giữ được sự đảm bảo đó. 

Dữ liệu đầu vào đưa ra số lượng đội, tiếp theo là trình độ kỹ năng của họ, tiếp theo là mười vấn đề khó khăn hiện có. Đầu ra là độ khó lớn nhất mà chúng ta có thể gán cho bài toán mới. 

Những hạn chế nhỏ một cách bất thường. Có nhiều nhất là 32 đội và chỉ có 10 vấn đề hiện tại, do đó, ngay cả việc tìm kiếm toàn diện trên tất cả mười khó khăn có thể xảy ra, kiểm tra từng đội đối với mọi vấn đề hiện có, cũng chỉ đạt hiệu quả tối đa. 

[ 
10 \times 32 \times 10 = 3200 
] 

những so sánh cơ bản Đó là rất nhỏ cho giới hạn 1 giây. Một giải pháp trực tiếp hơn có thể thực hiện công việc chỉ với (10n) so sánh và không cần cấu trúc dữ liệu phức tạp, sắp xếp hoặc tìm kiếm nhị phân. 

Các trường hợp nguy hiểm chính đến từ các đội đã được bảo hiểm và các đội chưa được bảo hiểm. Hãy xem xét đầu vào này:```
1
1
2 3 4 5 6 7 8 9 10 10
```Đội duy nhất có kỹ năng 1 và không thể giải quyết bất kỳ vấn đề nào hiện có. Bài toán mới phải có độ khó nhiều nhất là 1, nên câu trả lời là 1. Việc triển khai bất cẩn chỉ lấy độ khó hiện có tối thiểu sẽ trả về 2, mặc dù bài toán đó cũng quá khó đối với nhóm. 

Tình huống ngược lại cũng dễ xử lý sai:```
1
5
1 2 3 4 5 6 7 8 9 10
```Đội đã giải được bài toán khó 1 rồi nên bài toán mới không cần giúp đỡ đội này chút nào. Chúng tôi có thể làm cho nó khó đến mức cho phép, đưa ra câu trả lời là 10. Việc triển khai luôn hạn chế vấn đề mới ở mức kỹ năng nhóm tối thiểu sẽ trả về sai 5. 

Ngoài ra còn có một trường hợp ranh giới trong đó một vấn đề hiện tại có chính xác kỹ năng của nhóm:```
1
5
5 6 7 8 9 10 10 10 10 10
```Đội có thể giải được độ khó 5, vì điều kiện là độ khó nhỏ hơn hoặc bằng kỹ năng. Nó đã được che rồi nên câu trả lời là 10. Sử dụng phép so sánh chặt chẽ như`difficulty < skill`sẽ phân loại sai nhóm là chưa được khám phá. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể thử mọi độ khó mới có thể có từ 1 đến 10. Đối với mỗi ứng cử viên, nó sẽ kiểm tra mọi đội. Nếu đội có thể giải quyết được vấn đề mới, đội đó sẽ được bảo vệ. Mặt khác, quá trình triển khai sẽ quét mười vấn đề hiện có và kiểm tra xem liệu có ít nhất một vấn đề có thể giải quyết được hay không. Một ứng cử viên chỉ hợp lệ khi mọi đội đều có mặt và ứng cử viên hợp lệ lớn nhất là câu trả lời. 

Phương pháp này hoàn toàn chính xác vì chỉ có mười giá trị có thể có cho độ khó mới nên việc kiểm tra tất cả chúng không thể bỏ sót giá trị tối ưu. Với (n \leq 32), trường hợp xấu nhất của nó chỉ là phép so sánh (10 \times 32 \times 10 = 3200). Vì vậy, mặc dù là cách tiếp cận bạo lực, nhưng nó vẫn đủ nhanh để đáp ứng các ràng buộc thực tế. Không có kích thước đầu vào nào mà tại đó lực lượng vũ phu cụ thể này trở nên quá chậm trong giới hạn đã nêu. 

Chúng ta vẫn có thể đơn giản hóa lý luận một cách đáng kể. Đối với một nhóm cố định, chỉ có vấn đề dễ nhất hiện có mới quan trọng. Đặt độ khó hiện tại tối thiểu đó là (m). Nếu (m \leq s), trong đó (s) là kỹ năng của đội, thì đội đã được bảo vệ và không đặt ra hạn chế nào đối với vấn đề mới. Nếu (m > s) thì không có bài toán hiện tại nào có thể giải được, do đó bài toán mới chỉ có nhiều nhất là có độ khó (s). 

Điều này có nghĩa là chúng tôi không cần phải kiểm tra mười độ khó của ứng viên. Chúng tôi có thể kiểm tra từng đội một lần, xác định xem vấn đề hiện tại tối thiểu của họ có thể giải quyết được hay không và đối với mỗi đội chưa được khám phá, hãy ghi lại kỹ năng của họ. Vấn đề mới phải được giải quyết bởi mọi đội chưa được khám phá nên độ khó của nó không thể vượt quá kỹ năng nhỏ nhất trong số họ. 

Nếu không có đội nào chưa được khám phá, bài toán mới có thể có độ khó tối đa cho phép là 10. Ngược lại, câu trả lời chính xác là kỹ năng tối thiểu trong số các đội chưa được khám phá. 

Brute-force hoạt động vì phạm vi ứng cử viên rất nhỏ, nhưng việc quan sát thấy rằng mỗi đội chỉ bị ràng buộc bởi vấn đề dễ tồn tại nhất của nó cho phép chúng tôi thu gọn toàn bộ quá trình tìm kiếm thành một lần vượt qua các đội. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10 · n · 10) = O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(10 · n) = O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc trình độ kỹ năng của mỗi đội và mười vấn đề khó khăn hiện có. 
2. Tìm độ khó tối thiểu trong số các bài toán hiện có. Gọi nó`easiest`. 

Đối với bất kỳ đội nào, nếu`easiest`lớn hơn kỹ năng của mình thì mọi vấn đề hiện tại đều quá khó khăn đối với đội đó. Ngược lại, nếu`easiest`nhiều nhất là kỹ năng của mình, nhóm đó đã có thể giải quyết được ít nhất một vấn đề hiện có. 
3. Đặt câu trả lời ban đầu là 10. 

Nếu mọi đội đều đã được giải xong thì không có hạn chế nào đối với vấn đề mới, do đó độ khó tối đa cho phép là 10 là đúng. 
4. Đối với mỗi đội có kỹ năng nhỏ hơn`easiest`, hãy cập nhật câu trả lời thành câu trả lời nhỏ hơn so với câu trả lời hiện tại và kỹ năng của đội đó. 

Một đội như vậy không thể giải quyết được bất kỳ vấn đề cũ nào, vì vậy vấn đề mới phải có độ khó không lớn hơn kỹ năng của đội đó. Vì vấn đề mới phải phù hợp với mọi nhóm như vậy nên chúng tôi cần mức độ kỹ năng nhỏ nhất của họ. 
5. In câu trả lời có được. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`easiest`là độ khó tối thiểu trong số tất cả các vấn đề hiện có. Đối với mỗi đội có kỹ năng ít nhất`easiest`, bài toán dễ nhất đó có thể giải được, vì vậy nhóm đã có sẵn một bài toán hợp lệ và không áp đặt điều kiện nào cho bài toán mới. Dành cho mọi đội có kỹ năng dưới đây`easiest`, ngay cả vấn đề hiện tại dễ nhất cũng quá khó, vì vậy vấn đề mới là vấn đề duy nhất có thể giải được của họ và độ khó của nó nhiều nhất phải nằm ở kỹ năng của họ. Do đó, độ khó mới không thể lớn hơn kỹ năng tối thiểu trong số tất cả các đội chưa được khám phá và việc chọn chính xác giá trị đó sẽ làm hài lòng mọi đội chưa được khám phá trong khi càng khó càng tốt. 

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

if __name__ == "__main__":
    solve()
```Hai dòng đầu vào đầu tiên cung cấp cho chúng ta kỹ năng làm việc nhóm và những khó khăn của vấn đề hiện có. Vì có đúng mười vấn đề,`min(difficulties)`ngay lập tức cung cấp thông tin quan trọng duy nhất về vấn đề hiện tại. 

Câu trả lời bắt đầu từ số 10 vì 10 là độ khó mới lớn nhất có thể. Chúng tôi chỉ hạ thấp nó khi tìm thấy một nhóm không thể giải quyết bất kỳ vấn đề nào hiện có. 

Sự so sánh là`skill < easiest`, không`skill <= easiest`. Một đội có kỹ năng tương đương với độ khó của bài toán dễ nhất có thể giải được bài toán đó nên bài toán đó đã được giải quyết xong. 

Không thể tràn số nguyên vì mọi giá trị đều nằm trong khoảng từ 1 đến 10. Thuật toán cũng không cần sắp xếp hoặc thêm mảng ngoài mảng đầu vào, giúp việc triển khai ở mức nhỏ gọn và tránh những công việc không cần thiết. 

Đầu vào chỉ chứa một ca kiểm thử, do đó không có vòng lặp ca kiểm thử bên ngoài. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
4
3 7 5 5
4 6 5 7 4 4 9 10 7 9
```Bài toán dễ nhất hiện có lại có độ khó 4. 

| Kỹ năng đồng đội | Vấn đề dễ nhất | Đã được bảo hiểm? | Trả lời sau đội | 
| --- | --- | --- | --- | 
| 3 | 4 | Không | 3 | 
| 7 | 4 | Có | 3 | 
| 5 | 4 | Có | 3 | 
| 5 | 4 | Có | 3 | 

Đội có kỹ năng 3 không thể giải được bất kỳ vấn đề nào hiện có nên vấn đề mới phải có độ khó tối đa là 3. Mọi đội khác đều đã giải được độ khó 4 nên không áp đặt giới hạn nhỏ hơn. Câu trả lời hợp lệ tối đa là 3. 

### Xây dựng ví dụ 2 

Không có mẫu chính thức thứ hai trong tuyên bố được cung cấp, vì vậy hãy xem xét:```
5
4 6 7 10 8
5 6 9 10 10 10 10 10 10 10
```Bài toán hiện có dễ nhất lại có độ khó 5. 

| Kỹ năng đồng đội | Vấn đề dễ nhất | Đã được bảo hiểm? | Trả lời sau đội | 
| --- | --- | --- | --- | 
| 4 | 5 | Không | 4 | 
| 6 | 5 | Có | 4 | 
| 7 | 5 | Có | 4 | 
| 10 | 5 | Có | 4 | 
| 8 | 5 | Có | 4 | 

Chỉ có đội có kỹ năng 4 mới được khám phá. Do đó, vấn đề mới phải có nhiều nhất là 4 và độ khó 4 phù hợp với đội đó. Câu trả lời là 4. 

Dấu vết này chứng tỏ tại sao chúng ta chỉ quan tâm đến vấn đề dễ nhất hiện có. Một khi một nhóm không thể giải quyết được vấn đề đó thì nó cũng không thể giải quyết được bất kỳ vấn đề nào khác bởi vì ít nhất tất cả chúng đều khó khăn như nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc tìm điểm nhỏ nhất trong số mười bài toán mất O(10), sau đó mỗi đội được kiểm tra một lần. | 
| Không gian | O(n) | Các kỹ năng của nhóm được lưu trữ trong một mảng; những khó khăn của bài toán sử dụng một mảng có kích thước không đổi gồm mười giá trị. | 

Với tối đa 32 nhóm, thuật toán chỉ thực hiện vài chục thao tác có ý nghĩa sau khi đọc dữ liệu đầu vào. Nó thấp hơn nhiều so với giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    skills = list(map(int, input().split()))
    difficulties = list(map(int, input().split()))

    easiest = min(difficulties)
    answer = 10

    for skill in skills:
        if skill < easiest:
            answer = min(answer, skill)

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided sample
assert run(
    """4
3 7 5 5
4 6 5 7 4 4 9 10 7 9
"""
) == "3\n", "sample 1"

# Minimum-size input, the only team cannot solve any existing problem.
assert run(
    """1
1
2 3 4 5 6 7 8 9 10 10
"""
) == "1\n", "minimum size"

# Everyone is already covered, so the new problem can have difficulty 10.
assert run(
    """1
5
1 2 3 4 5 6 7 8 9 10
"""
) == "10\n", "already covered"

# Equality boundary: skill exactly equals the easiest problem.
assert run(
    """3
5 6 10
5 7 8 9 10 10 10 10 10 10
"""
) == "10\n", "equality boundary"

# Maximum number of teams, with several uncovered teams.
assert run(
    """32
1 1 1 1 1 1 2 2 2 2 3 3 3 3 4 4
5 5 5 5 5 5 5 5 5 5
"""
) == "1\n", "maximum n"

# All teams have the same skill and all existing problems are too difficult.
assert run(
    """8
7 7 7 7 7 7 7 7
8 8 8 9 9 10 10 10 10 10
"""
) == "7\n", "all equal skills"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 2 3 4 5 6 7 8 9 10 10`|`1`| Đầu vào có kích thước tối thiểu và câu trả lời thấp nhất có thể | 
|`1 / 5 / 1 2 3 4 5 6 7 8 9 10`|`10`| Mọi đội đều đã được bảo hiểm | 
|`3 / 5 6 10 / 5 7 8 9 10 10 10 10 10 10`|`10`| Độ khó tương đương với kỹ năng có thể giải quyết được | 
|`32 / ... / ten problems all at least 5`|`1`| Số lượng đội tối đa và kỹ năng chưa được phát hiện rất thấp | 
|`8 / all skills 7 / problems all at least 8`|`7`| Tất cả các đội đều có những ràng buộc giống hệt nhau | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là nhóm nhỏ nhất có thể không có vấn đề nào có thể giải quyết được:```
1
1
2 3 4 5 6 7 8 9 10 10
```Đây`easiest = 2`. Kỹ năng của đội là 1, vì vậy`1 < 2`và câu trả lời trở thành`min(10, 1) = 1`. Thuật toán in`1`. Điều này nắm bắt các triển khai gây nhầm lẫn giữa độ khó tối thiểu hiện có với độ khó mới bắt buộc. 

Trường hợp bên thứ hai có một nhóm đã được bảo vệ:```
1
5
1 2 3 4 5 6 7 8 9 10
```Đây`easiest = 1`, và kỹ năng 5 của đội không nhỏ hơn 1. Đội bị bỏ qua vì đã giải được bài toán khó-1. Câu trả lời vẫn là 10, đây là độ khó mới tối đa có thể có. 

Ranh giới bình đẳng hoạt động tương tự:```
3
5 6 10
5 7 8 9 10 10 10 10 10 10
```Bài toán dễ nhất có độ khó 5. Đội đầu tiên có kỹ năng đúng 5 nên có thể giải được bài toán đó. Các đội khác cũng có thể giải quyết được. Không có đội nào bị phát hiện và thuật toán giữ nguyên câu trả lời ban đầu là 10. Điều này xác nhận rằng điều kiện giải được phải sử dụng`<=`, được biểu thị bằng phép thử chưa được khám phá`skill < easiest`. 

Cuối cùng, hãy xem xét một số nhóm không thể giải quyết bất kỳ vấn đề hiện có nào:```
5
3 7 2 8 4
5 6 7 8 9 10 10 10 10 10
```Bài toán hiện tại dễ nhất có độ khó 5. Các đội có kỹ năng 3, 2 và 4 chưa được khám phá nên bài toán mới phải có độ khó tối đa là 3, tối đa là 2 và nhiều nhất là 4 cùng một lúc. Giới hạn chặt chẽ nhất là 2, vì vậy thuật toán sẽ cập nhật câu trả lời khi gặp các đội này và kết thúc với`2`. Một vấn đề mới ở độ khó 2 đều có thể giải được bởi cả ba đội chưa được khám phá, trong khi độ khó 3 sẽ không thành công đối với đội có kỹ năng 2.
