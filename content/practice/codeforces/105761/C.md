---
title: "CF 105761C - Bàn đứng bóng đá"
description: "Chúng ta được cấp năm số nguyên mô tả số liệu thống kê về mùa giải của một đội bóng đá, nhưng thứ tự của các số nguyên này bị xáo trộn. Chúng tôi biết chúng tương ứng với các trận đấu đã chơi, thắng, hòa, thua và tổng điểm, nhưng chúng tôi không biết đó là số nào."
date: "2026-06-21T23:50:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105761
codeforces_index: "C"
codeforces_contest_name: "2021 UCF Local Programming Contest"
rating: 0
weight: 105761
solve_time_s: 43
verified: true
draft: false
---

[CF 105761C - Bàn đứng bóng đá](https://codeforces.com/problemset/problem/105761/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp năm số nguyên mô tả số liệu thống kê về mùa giải của một đội bóng đá, nhưng thứ tự của các số nguyên này bị xáo trộn. Chúng tôi biết chúng tương ứng với các trận đấu đã chơi, thắng, hòa, thua và tổng điểm, nhưng chúng tôi không biết đó là số nào. 

Luật tính điểm bóng đá được cố định: thắng được 3 điểm, hòa được 1 điểm, thua được 0 điểm. Ngoài ra, mỗi trận đấu đều dẫn đến chính xác một trong các kết quả này, vì vậy tổng số trận đấu đã chơi là tổng số trận thắng, trận hòa và trận thua. Tổng số điểm được tính bằng ba lần thắng cộng với hòa. 

Nhiệm vụ là lấy năm số theo thứ tự chưa biết và xây dựng lại cách diễn giải hợp lệ duy nhất, sau đó in chúng theo thứ tự chuẩn: MP, W, D, L, Pts. 

Mặc dù tất cả các giá trị đều nhỏ, lên tới 300, nhưng khó khăn chính là chúng ta phải phân công chính xác các vai trò theo hai ràng buộc cùng một lúc: ràng buộc tuyến tính từ số trận đấu và một ràng buộc khác từ tính điểm. Một nỗ lực ngây thơ gán vai trò một cách tùy ý sẽ thất bại vì tồn tại nhiều hoán vị, nhưng chỉ có một hoán vị thỏa mãn cả hai phương trình. 

Các trường hợp cạnh chủ yếu xoay quanh các giá trị lặp lại. Ví dụ: một đầu vào như`0 0 1 1 0`chứa các bản sao, làm cho phép gán tham lam ngây thơ trở nên mơ hồ. Một trường hợp tinh tế khác là khi các trận hòa và thua hoặc thắng và hòa có giá trị giống hệt nhau, điều này có thể gây hiểu lầm cho phương pháp phỏng đoán dựa trên sắp xếp mặc dù giải pháp hợp lệ vẫn là duy nhất. 

Vì chỉ có năm số nên việc sử dụng mạnh mẽ tất cả các hoán vị là khả thi trong thời gian không đổi. Đó là quan sát cấu trúc chính. 

## Phương pháp tiếp cận 

Một ý tưởng trực tiếp là thử tất cả các phép gán có thể có của năm số cho các vai trò MP, W, D, L và Pts. Đối với mỗi bài tập, chúng tôi kiểm tra xem nó có thỏa mãn hai điều kiện hay không: thứ nhất, MP phải bằng W + D + L và thứ hai, Pts phải bằng 3W + D. Nếu cả hai đều đúng, chúng tôi đã tìm ra cách giải thích đúng. 

Chỉ có năm giá trị nên số hoán vị là 5 giai thừa, tức là 120. Với mỗi hoán vị, chúng ta thực hiện công không đổi nên cách tiếp cận này cực kỳ nhanh. 

Việc tối ưu hóa “dựa trên suy nghĩ” nhiều hơn là không cần thiết ở đây vì các ràng buộc rất nhỏ. Ngay cả khi chúng tôi cố gắng giảm nó hơn nữa bằng cách suy luận xem số nào lớn nhất hay nhỏ nhất, chúng tôi vẫn cần xác minh tính nhất quán và việc kiểm tra hoán vị vốn đã đơn giản và an toàn. 

Cách tiếp cận bạo lực hoạt động vì không gian tìm kiếm rất nhỏ và các ràng buộc xác định đầy đủ một ánh xạ duy nhất. Việc quan sát chỉ có 5! các bài tập tồn tại sẽ biến những gì trông giống như một vấn đề lý luận thành một vòng lặp xác thực đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(5!) | O(1) | Đã chấp nhận | 
| Tối ưu (giống như vũ phu) | O(120) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi năm số đầu vào là một tập hợp nhiều số và thử mọi phép gán có thể có cho năm vai trò. 

1. Đọc năm số nguyên thành một mảng. Chúng đại diện cho một hoán vị không xác định của MP, W, D, L và Pts. 
2. Tạo tất cả các hoán vị của năm giá trị này. Mỗi hoán vị tương ứng với một phép gán giả định: giá trị đầu tiên là MP, W thứ hai, D thứ ba, L thứ tư và Pts thứ năm. Chúng tôi không giả định bất kỳ thứ tự nào vì đầu vào không đưa ra gợi ý nào. 
3. Với mỗi hoán vị, hãy tính xem nó có thỏa mãn các ràng buộc về cấu trúc của thống kê bóng đá hay không. Chúng tôi kiểm tra hai phương trình: MP phải bằng W + D + L và Pts phải bằng 3W + D. Bước này là điều kiện lọc duy nhất và nó thực thi các quy tắc của hệ thống tính điểm. 
4. Bài toán đảm bảo tồn tại chính xác một phép gán hợp lệ, vì vậy ngay khi tìm thấy một hoán vị thỏa mãn cả hai điều kiện, chúng ta xuất nó theo thứ tự yêu cầu và kết thúc. 

Lý do điều này là đủ vì các ràng buộc đã xác định đầy đủ hệ thống. Bất kỳ phép gán sai nào cũng sẽ vi phạm ít nhất một phương trình, vì vậy nó không thể vượt qua bài kiểm tra. 

### Tại sao nó hoạt động 

Mỗi hàng trong bảng bóng đá hợp lệ được xác định đầy đủ bởi hai phương trình MP = W + D + L và Pts = 3W + D. Vì chúng tôi đang kiểm tra tất cả các phép gán có thể có của các số đầu vào vào các vai trò này nên cấu hình chính xác phải xuất hiện trong số các hoán vị. Sự đảm bảo tính duy nhất đảm bảo rằng không có hoán vị nào khác có thể thỏa mãn đồng thời cả hai ràng buộc, vì vậy kết quả khớp hợp lệ đầu tiên mà chúng tôi tìm thấy là câu trả lời duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from itertools import permutations

nums = list(map(int, input().split()))

for mp, w, d, l, pts in permutations(nums):
    if mp == w + d + l and pts == 3 * w + d:
        print(mp, w, d, l, pts)
        break
```Giải pháp trực tiếp mã hóa ý tưởng kiểm tra nhiệm vụ toàn diện. Chúng tôi dựa vào Python`itertools.permutations`để liệt kê tất cả các nhiệm vụ vai trò có thể. 

Chi tiết quan trọng là chúng tôi không cố gắng tối ưu hóa thứ tự lựa chọn hoặc cắt tỉa một cách mạnh mẽ. Chỉ với 120 trường hợp, chi phí không đáng kể. Việc nghỉ sớm đảm bảo chúng tôi dừng ngay lập tức khi tìm thấy cấu hình hợp lệ, phù hợp với đảm bảo tính duy nhất. 

Một điểm tinh tế là chúng tôi xử lý tất cả các hoán vị như nhau, ngay cả khi các giá trị lặp lại. Điều này là tốt vì tính chính xác phụ thuộc vào việc gán giá trị chứ không phải nhận dạng chỉ mục. Các giá trị trùng lặp có thể tạo ra các hoán vị trùng lặp nhưng chúng không ảnh hưởng đến tính chính xác hoặc hiệu suất theo bất kỳ cách nào có ý nghĩa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`19 11 2 4 5`Chúng tôi kiểm tra các hoán vị cho đến khi tìm thấy ánh xạ hợp lệ. 

| nghị sĩ | W | D | L | Điểm | Kiểm tra MP | Kiểm tra điểm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 11 | 5 | 4 | 2 | 19 | 11 ≠ 11 | 19 = 19 | 

Ở đây, MP = 11, W = 5, D = 4, L = 2, Pts = 19. 

Chúng tôi xác minh MP = W + D + L → 11 = 5 + 4 + 2 = 11 và Pts = 3W + D → 19 = 15 + 4 = 19. 

Điều này xác nhận sự phân công chính xác được tìm thấy. 

### Ví dụ 2 

đầu vào:`0 0 1 1 0`Một hoán vị hợp lệ là: 

| nghị sĩ | W | D | L | Điểm | Kiểm tra MP | Kiểm tra điểm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 0 | 1 = 1 | 0 = 0 | 

Ở đây MP = 1, W = 0, D = 0, L = 1, Pts = 0 thỏa mãn chính xác cả hai ràng buộc. 

Trường hợp này cho thấy tại sao các giá trị trùng lặp lại quan trọng: tồn tại nhiều hoán vị nhưng chỉ có một hoán vị thỏa mãn cả hai phương trình cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(5!) | Chúng tôi kiểm tra tất cả các hoán vị của năm phần tử, mỗi lần kiểm tra là O(1) | 
| Không gian | O(1) | Chỉ có các biến phụ không đổi ngoài bộ nhớ đầu vào | 

Kích thước đầu vào được cố định ở năm số nguyên, do đó, ngay cả việc liệt kê bạo lực cũng có hiệu quả ngay lập tức. Giải pháp dễ dàng phù hợp với mọi hạn chế về thời gian. 

## Trường hợp thử nghiệm```python
import sys, io
from itertools import permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    nums = list(map(int, sys.stdin.readline().split()))
    for mp, w, d, l, pts in permutations(nums):
        if mp == w + d + l and pts == 3 * w + d:
            return f"{mp} {w} {d} {l} {pts}"
    return ""

# provided samples
assert run("19 11 2 4 5") == "11 5 4 2 19"
assert run("2 2 6 20 10") == "10 6 2 2 20"
assert run("0 0 1 1 0") == "1 0 0 1 0"

# custom cases
assert run("3 1 1 1 4") == "3 1 1 1 4", "simple valid case"
assert run("9 3 3 3 12") == "9 3 3 3 12", "balanced draws case"
assert run("6 2 1 3 7") == "6 2 1 3 7", "mixed ordering case"
assert run("1 0 1 0 1") == "1 0 1 0 1", "edge small values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 1 1 4 | 3 1 1 1 4 | tái thiết hợp lệ cơ bản | 
| 9 3 3 3 12 | 9 3 3 3 12 | trường hợp kéo nặng đối xứng | 
| 6 2 1 3 7 | 6 2 1 3 7 | xáo trộn vai trò đúng đắn | 
| 1 0 1 0 1 | 1 0 1 0 1 | cấu hình cạnh tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một số giá trị giống hệt nhau, điều này làm tăng số lượng hoán vị không thể phân biệt được. Đối với đầu vào như`0 0 1 1 0`, thuật toán vẫn hoạt động vì tính đúng đắn không gắn liền với tính duy nhất của các giá trị mà gắn với sự thỏa mãn các ràng buộc. Vòng lặp hoán vị cuối cùng sẽ thử`MP=1, W=0, D=0, L=1, Pts=0`, vượt qua cả hai lần kiểm tra và tất cả các hoán vị khác đều vi phạm tính nhất quán của MP hoặc tính nhất quán của điểm. 

Một trường hợp khác là khi chiến thắng bằng 0. Đối với đầu vào như`1 0 0 1 0`, cách giải thích hợp lệ không có đóng góp tính điểm từ các trận thắng hoặc trận hòa ngoại trừ cấu trúc. Việc kiểm tra vẫn thành công vì MP = 0 + 0 + 1 và Pts = 0 khớp chính xác. 

Trường hợp cạnh thứ ba là khi các trận hòa bằng 0, kết quả là tính điểm thành bội số thắng thuần túy. Ví dụ: nếu đầu vào là`6 2 0 4 6`, chỉ có hoán vị đúng mới thỏa mãn đồng thời cả hai phương trình MP và Pts. Thuật toán không xử lý số 0 một cách đặc biệt, nó chỉ đơn giản xác nhận các phương trình một cách thống nhất, giúp tránh mọi lỗi trong trường hợp đặc biệt.
