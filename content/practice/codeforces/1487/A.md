---
title: "CF 1487A - Đấu trường"
description: "Chúng ta được cung cấp một tập hợp các anh hùng, mỗi anh hùng có sức mạnh hoặc cấp độ ban đầu. Họ chiến đấu theo cặp, và bất cứ khi nào hai anh hùng có cấp độ khác nhau gặp nhau, người mạnh hơn sẽ chiến thắng và ngày càng mạnh hơn. Nếu cấp độ của họ bằng nhau, trận đấu sẽ hòa và không ai giành được gì cả."
date: "2026-06-10T22:58:38+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1487
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 104 (Rated for Div. 2)"
rating: 800
weight: 1487
solve_time_s: 130
verified: true
draft: false
---

[CF 1487A - Đấu trường](https://codeforces.com/problemset/problem/1487/A) 

**Đánh giá:** 800 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các anh hùng, mỗi anh hùng có sức mạnh hoặc cấp độ ban đầu. Họ chiến đấu theo cặp, và bất cứ khi nào hai anh hùng có cấp độ khác nhau gặp nhau, người mạnh hơn sẽ chiến thắng và ngày càng mạnh hơn. Nếu cấp độ của họ bằng nhau, trận đấu sẽ hòa và không ai giành được gì cả. Chúng ta được yêu cầu tìm xem có bao nhiêu anh hùng có cơ hội trở thành nhà vô địch cuối cùng nếu chúng ta có thể chọn thứ tự chiến đấu một cách tùy ý. Nói cách khác, một anh hùng có thể là người chiến thắng nếu tồn tại một chuỗi trận chiến trong đó anh hùng này giành chiến thắng trong một số lượng lớn các cuộc đấu tay đôi. 

Đầu vào cung cấp số lượng anh hùng và cấp độ ban đầu của họ. Đầu ra là số lượng anh hùng có khả năng giành chiến thắng trong một số chuỗi trận chiến. Với$n$lên đến$100$và mức độ giới hạn ở$100$, chúng ta có đủ khả năng để xem xét từng anh hùng một cách riêng lẻ và suy luận về các cấp độ tương đối. Mô phỏng bạo lực của tất cả các chuỗi có thể là không thực tế vì số lượng các chuỗi tăng lên một cách bùng nổ. 

Một trường hợp khó nhận thấy là khi tất cả các anh hùng đều có cấp độ ngang nhau. Trong kịch bản đó, trận nào cũng hòa nên không anh hùng nào có thể tích lũy được chiến thắng. Ví dụ, với mức độ`[5, 5]`, câu trả lời là`0`. Một trường hợp khác xảy ra khi chỉ có một anh hùng mạnh hơn những anh hùng khác; chỉ có anh hùng đó mới có thể giành chiến thắng. Vì`[3, 2, 2]`, chỉ có anh hùng mạnh nhất mới có thể là người chiến thắng. Những cách tiếp cận ngây thơ có thể coi không chính xác những anh hùng có cấp độ cao thứ hai là người chiến thắng vì họ có thể thắng những trận chiến nhỏ, nhưng trên thực tế, họ không bao giờ có thể vượt qua anh hùng mạnh nhất theo trình tự được tối ưu hóa cho người chiến thắng cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi trận đánh. Đối với mỗi anh hùng, chúng tôi có thể thử mọi cặp đấu tay đôi có thể và theo dõi chiến thắng. Về nguyên tắc điều này đúng, nhưng số lượng trận đấu cần thiết là rất lớn (`100^500`), do đó mọi mô phỏng thực tế đều không thể thực hiện được. Số lượng hoạt động sẽ bùng nổ ngay cả đối với`n = 100`, làm cho nó không thể thực hiện được. 

Quan sát quan trọng là một anh hùng chỉ có thể là người chiến thắng nếu cấp độ của họ không hoàn toàn nhỏ hơn cấp độ nhỏ nhất của bất kỳ đối thủ tiềm năng nào cuối cùng có thể vượt qua họ. Cụ thể, anh hùng cấp tối thiểu không bao giờ có thể ngăn cản chiến thắng của một anh hùng mạnh hơn và một anh hùng có cấp độ thấp hơn mức tối đa không thể ngăn cản anh hùng cấp tối đa cuối cùng giành chiến thắng. Vì vậy, chúng ta chỉ cần xem xét anh hùng có cấp độ tối thiểu. Anh hùng có khả năng là người chiến thắng nếu cấp độ của họ lớn hơn hoặc bằng cấp độ tối thiểu trong mảng. 

Cái nhìn sâu sắc này làm giảm vấn đề trong việc tìm cấp độ nhỏ nhất và đếm những anh hùng có cấp độ lớn hơn`min_level`. Nếu tất cả các anh hùng bị ràng buộc ở mức tối thiểu, không ai có thể giành chiến thắng, điều này phù hợp với trường hợp lợi thế đã thảo luận ở trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ơ(???) | O(n) | Không khả thi do số lượng trận đánh quá lớn | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy xử lý các bước sau. 
2. Đọc`n`, số lượng anh hùng và cấp độ của họ`a`. 
3. Xác định cấp độ tối thiểu của tất cả các anh hùng. Cấp độ này đại diện cho anh hùng “yếu nhất” có thể tham gia vào chuỗi chiến thắng. 
4. Đếm tất cả các anh hùng có cấp độ cao hơn mức tối thiểu này. Những anh hùng này là ứng cử viên vì họ luôn có thể thống trị những anh hùng cấp tối thiểu cho đến phút cuối cùng. 
5. In số lượng của ca kiểm thử hiện tại. 

Tại sao nó hoạt động: Một anh hùng không thể trở thành người chiến thắng nếu tồn tại một anh hùng mạnh hơn, bởi vì cuối cùng anh hùng đó sẽ luôn vượt qua họ về số trận thắng. Ngược lại, bất kỳ anh hùng nào có cấp độ trên mức tối thiểu đều có thể, trong một số chuỗi trận chiến, loại bỏ hoặc dẫn trước những anh hùng yếu hơn và tích lũy số chiến thắng cần thiết. Điều này làm giảm vấn đề xuống mức so sánh đơn giản với mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    min_level = min(a)
    count = sum(1 for x in a if x > min_level)
    print(count)
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc các cấp độ chính và tính toán mức tối thiểu. Sau đó chúng tôi đếm những anh hùng mạnh hơn mức tối thiểu. Việc sử dụng biểu thức trình tạo sẽ tránh được danh sách trung gian không cần thiết. Điều này đảm bảo xử lý O(n) cho mỗi trường hợp thử nghiệm, hiệu quả với các ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[3, 2, 2]`| Anh hùng | Cấp độ | Cấp độ > cấp độ tối thiểu? | 
| --- | --- | --- | 
| 1 | 3 | Có | 
| 2 | 2 | Không | 
| 3 | 2 | Không | 

Mức tối thiểu là`2`. Chỉ có anh hùng có cấp độ`3`vượt quá mức tối thiểu này, vì vậy có thể có 1 người chiến thắng. 

### Ví dụ 2 

đầu vào:`[5, 5]`| Anh hùng | Cấp độ | Cấp độ > cấp độ tối thiểu? | 
| --- | --- | --- | 
| 1 | 5 | Không | 
| 2 | 5 | Không | 

Tất cả các anh hùng đều có cùng cấp độ. Tối thiểu là`5`và không có anh hùng nào mạnh hơn hoàn toàn, vì vậy số lượng là`0`. 

### Ví dụ 3 

đầu vào:`[1, 3, 3, 7]`| Anh hùng | Cấp độ | Cấp độ > cấp độ tối thiểu? | 
| --- | --- | --- | 
| 1 | 1 | Không | 
| 2 | 3 | Có | 
| 3 | 3 | Có | 
| 4 | 7 | Có | 

Tối thiểu là`1`. Ba anh hùng thực sự mạnh hơn nên câu trả lời là`3`. 

Những dấu vết này xác nhận thuật toán tính chính xác những anh hùng mạnh hơn những người yếu nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp kiểm thử quét n phần tử hai lần (tối thiểu và đếm). Tối đa t = 500, n = 100 → 50.000 thao tác. | 
| Không gian | O(n) | Chúng tôi lưu trữ các cấp độ trong một mảng cho từng trường hợp thử nghiệm. | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        min_level = min(a)
        count = sum(1 for x in a if x > min_level)
        print(count)
    return output.getvalue().strip()

# provided samples
assert run("3\n3\n3 2 2\n2\n5 5\n4\n1 3 3 7\n") == "1\n0\n3", "sample 1"

# custom cases
assert run("1\n2\n1 100\n") == "1", "one very strong, one weak"
assert run("1\n5\n10 10 10 10 10\n") == "0", "all equal levels"
assert run("1\n4\n1 2 3 4\n") == "3", "all different levels"
assert run("1\n3\n2 2 3\n") == "1", "single winner above duplicates"
assert run("1\n2\n100 1\n") == "1", "max and min boundary values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n1 100`|`1`| Một anh hùng mạnh mẽ thống trị anh hùng yếu | 
|`5\n10 10 10 10 10`|`0`| Tất cả đều bình đẳng, không có người chiến thắng | 
|`4\n1 2 3 4`|`3`| Nhiều anh hùng mạnh hơn min | 
|`3\n2 2 3`|`1`| Chỉ mạnh nhất trong số các bản sao mới thắng | 
|`2\n100 1`|`1`| Xử lý giá trị biên | 

## Vỏ cạnh 

Nếu tất cả các anh hùng đều có cùng cấp độ thì mức tối thiểu bằng tất cả các cấp độ và không có anh hùng nào mạnh hơn hoàn toàn. Thuật toán trả về chính xác`0`. Đối với những anh hùng có một anh hùng đặc biệt mạnh và một số anh hùng yếu, thuật toán chỉ tính anh hùng mạnh là người có khả năng chiến thắng. Đối với mảng nhỏ (`n = 2`), thuật toán xử lý chính xác cả giá trị bằng nhau và không bằng nhau. Việc triển khai cũng hoạt động ở mức tối đa được phép (`100`) và số lượng anh hùng tối đa (`100`) không bị tràn hoặc kém hiệu quả.
