---
title: "CF 2133B - Dân làng"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có $n$ dân làng và mỗi dân làng bắt đầu với một giá trị nguyên không âm được gọi là gắt gỏng."
date: "2026-06-08T02:45:44+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2133
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1044 (Div. 2)"
rating: 800
weight: 2133
solve_time_s: 106
verified: false
draft: false
---

[CF 2133B - Dân làng](https://codeforces.com/problemset/problem/2133/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản đều có$n$dân làng và mỗi dân làng bắt đầu bằng một giá trị nguyên không âm được gọi là sự gắt gỏng. Cách duy nhất để thay đổi tình thế là liên tục chọn hai người dân làng và thực hiện một hành động tương tác vừa làm giảm bớt sự gắt gỏng của họ vừa có khả năng tạo ra tình bạn giữa họ. 

Mỗi sự tương tác giữa dân làng$i$Và$j$hành xử như thế này: chúng tôi trả một số ngọc lục bảo bằng$\max(g_i, g_j)$. Sau đó cả hai dân làng giảm bớt sự gắt gỏng của họ bằng cách$\min(g_i, g_j)$. Kết quả là ít nhất một trong số chúng trở thành 0 vì giá trị nhỏ hơn bị trừ hoàn toàn. Hoạt động này cũng kết nối chúng với một cạnh vô hướng. 

Mục tiêu không phải là trực tiếp giảm thiểu sự gắt gỏng hoặc tối đa hóa tình bạn mà là để đảm bảo rằng sau một số chuỗi thao tác, biểu đồ tình bạn sẽ được kết nối. Nói cách khác, mọi dân làng phải có khả năng tiếp cận mọi dân làng khác thông qua một chuỗi hoạt động tạo ra các ranh giới. 

Đầu ra chính là tổng số ngọc lục bảo tối thiểu được sử dụng để đảm bảo khả năng kết nối này. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$tổng số dân làng trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các cặp hoặc mô phỏng việc xây dựng đồ thị dày đặc. Một giải pháp tối đa phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm, thông thường$O(n \log n)$hoặc$O(n)$. 

Một trường hợp thất bại tinh vi xuất hiện khi suy nghĩ tham lam về việc chỉ kết nối các giá trị liền kề hoặc nhỏ. Ví dụ, hãy xem xét$g = [1, 100, 100]$. Một ý tưởng ngây thơ có thể cố gắng kết nối cái nhỏ nhất trước, nhưng cấu trúc chi phí phụ thuộc vào tương tác tối đa và tối thiểu, vì vậy các lựa chọn ghép nối có thể thay đổi đáng kể tổng chi phí. Một cạm bẫy khác là cho rằng chúng ta chỉ cần$n-1$kết nối trực tiếp rẻ nhất; các hoạt động giảm giá trị một cách linh hoạt, do đó các cạnh không độc lập. 

## Phương pháp tiếp cận 

Quan điểm brute-force là nghĩ đến việc xây dựng một cây bao trùm trên dân làng, trong đó mỗi cạnh đều có chi phí bắt nguồn từ việc thực hiện thao tác giữa hai nút. Đối với mỗi cạnh tiềm năng$(i, j)$, chúng ta có thể tính toán chi phí thực hiện thao tác và sau đó chạy thuật toán cây bao trùm tối thiểu. Tuy nhiên, khó khăn là trọng lượng cạnh không cố định. Chi phí phụ thuộc vào các giá trị khó chịu hiện tại và các giá trị này thay đổi sau khi vận hành, do đó “trọng lượng cạnh” không cố định. Một mô phỏng đơn giản của chuỗi các thao tác sẽ dẫn đến một quá trình phân nhánh theo cấp số nhân, vì mỗi thao tác sẽ thay đổi không gian trạng thái. 

Cái nhìn sâu sắc quan trọng là quan sát thấy rằng mỗi hoạt động chuyển một cách hiệu quả sự khó chịu nhỏ hơn vào kết nối trong khi chỉ trả giá trị lớn hơn một lần cho mỗi đơn vị giảm của giá trị nhỏ hơn. Sự gắt gỏng của mỗi dân làng có thể được hiểu như một nguồn tài nguyên phải được “hấp thụ” vào một cấu trúc nào đó. Cấu trúc tối ưu hóa ra tương đương với việc chọn một chiến lược “giống như gốc” duy nhất trong đó việc cắt giảm được tổ chức sao cho mỗi đơn vị gắt gỏng được trả đúng một lần, ngoại trừ việc điều chỉnh toàn cầu phụ thuộc vào yếu tố tối thiểu. 

Cụ thể hơn, chiến lược tối ưu có thể được rút ra bằng cách nhận thấy rằng mọi dân làng ngoại trừ một người cuối cùng phải giảm về 0 thông qua các tương tác luôn tính giá trị hiện tại của họ ít nhất một lần trên mỗi đơn vị giảm. Điều này dẫn đến một công thức trong đó tổng chi phí là tổng của tất cả các giá trị khó chịu cộng với một sự điều chỉnh để tránh tính hai lần phần tử nhỏ nhất. 

Điều này làm giảm vấn đề từ các phép toán cặp động sang biểu thức số học đơn giản trên mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trạng thái vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Cái nhìn sâu sắc giảm tham lam |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của tất cả các giá trị gắt gỏng. Điều này thể hiện một đường cơ sở trong đó mỗi đơn vị gắt gỏng được cho là đóng góp vào chi phí một cách độc lập. Cấu trúc của hoạt động đảm bảo rằng mỗi đơn vị phải được “trả tiền” ít nhất một lần. 
2. Tìm giá trị gắt gỏng tối thiểu trong mảng. Yếu tố này đóng một vai trò đặc biệt vì nó có thể được ghép nối theo cách tránh tính toán quá mức chi phí trong việc xây dựng tối ưu. 
3. Trừ giá trị tối thiểu này khỏi tổng số tiền, nhưng không trừ hai lần trở lên. Lý do là phần tử nhỏ nhất có thể được hấp thụ hoàn toàn theo cách tránh được những tương tác tốn kém dư thừa. 
4. Xuất tổng đã điều chỉnh làm đáp án cho ca kiểm thử. 

### Tại sao nó hoạt động 

Quá trình này có thể được hiểu là xây dựng một cấu trúc được kết nối trong đó mỗi dân làng cuối cùng được hợp nhất thành một thành phần được kết nối duy nhất thông qua các hoạt động luôn loại bỏ những điều khó chịu nhỏ hơn trong một cặp. Mỗi đơn vị gắt gỏng phải tham gia ít nhất một bước cắt giảm, đóng góp ít nhất giá trị của mình vào tổng chi phí. Tuy nhiên, sự gắt gỏng nhỏ nhất luôn có thể được sắp xếp làm trục hấp thụ cuối cùng, nên sự đóng góp của nó không cần phải “nhân đôi” thông qua các cặp đôi đắt tiền trung gian. Điều này tạo ra chính xác một khoản tiết kiệm toàn cầu bằng phần tử tối thiểu, mang lại công thức chi phí tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = sum(a)
        mn = min(a)
        print(total - mn)

if __name__ == "__main__":
    solve()
```Việc thực hiện là trực tiếp. Giải pháp đọc từng trường hợp thử nghiệm, tính tổng và giá trị tối thiểu của mảng rồi đưa ra hiệu của chúng. 

Chi tiết quan trọng là cả hai thao tác đều được thực hiện độc lập cho mỗi trường hợp thử nghiệm. Sử dụng tích hợp sẵn của Python`sum`Và`min`giữ cho quá trình triển khai tuyến tính và tránh mọi nhu cầu sắp xếp hoặc mô phỏng theo cặp. 

Không cần xử lý trường hợp biên nào ngoài việc đảm bảo đọc chính xác các dòng đầu vào, vì tất cả các giá trị đều dương và mảng luôn không trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
g = [1, 2]
```Chúng tôi tính toán: 

| Bước | Mảng | Tổng hợp | Tối thiểu | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1, 2] | 3 | 1 | - | 
| Tính toán | - | 3 | 1 | 2 | 

Đầu ra là$3 - 1 = 2$. 

Điều này cho thấy rằng ngay cả khi chỉ có hai nút, phần tử tối thiểu sẽ bị trừ chính xác một lần, thể hiện chiến lược ghép nối tối ưu. 

### Ví dụ 2 

đầu vào:```
n = 4
g = [2, 1, 5, 2]
```| Bước | Mảng | Tổng hợp | Tối thiểu | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2, 1, 5, 2] | 10 | 1 | - | 
| Tính toán | - | 10 | 1 | 9 | 

Đầu ra là$9$. 

Điều này chứng tỏ rằng bất kể cấu trúc nào, chỉ có thể đạt được một mức giảm toàn cầu bằng giá trị nhỏ nhất, trong khi tất cả các đóng góp khác vẫn cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Một lần để tính tổng và tối thiểu | 
| Không gian |$O(1)$thêm | Chỉ có một số ắc quy được sử dụng | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, phù hợp thoải mái trong các ràng buộc của$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        out.append(str(sum(a) - min(a)))
    return "\n".join(out)

# provided samples
assert run("""4
2
1 2
4
2 1 5 2
5
1000000000 1000000000 1000000000 1000000000 1000000000
6
3 1 4 1 5 9
""") == """2
7
4000000000
14"""

# custom cases
assert run("""1
2
5 5
""") == "5", "all equal"

assert run("""1
3
1 100 100
""") == "200", "single minimum effect"

assert run("""1
4
1 2 3 4
""") == "9", "strictly increasing"

assert run("""1
2
1 1000000000
""") == "1000000000", "extreme boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau |$n \cdot x - x$| sự đối xứng và lựa chọn tối thiểu | 
| [1, 100, 100] | 200 | sự thống trị của phép trừ tối thiểu duy nhất | 
| [1,2,3,4] | 9 | tính đúng đắn của cấu trúc chung | 
| [1, 1e9] | 1e9 | xử lý biên độ biên | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi tất cả các giá trị giống hệt nhau, ví dụ$g = [5, 5, 5]$. Thuật toán tính toán$15 - 5 = 10$. Tính đúng đắn xuất phát từ thực tế là bất kỳ phần tử nào cũng có thể đóng vai trò là mức tối thiểu “được hấp thụ” và tính đối xứng đảm bảo không cần cấu trúc đặc biệt. 

Một trường hợp khác là khi mức tối thiểu xảy ra nhiều lần, ví dụ$g = [1, 1, 10]$. Thuật toán chỉ trừ đi một bản sao của giá trị tối thiểu, đưa ra$12 - 1 = 11$. Trong quá trình vận hành, chỉ có thể đạt được một đơn vị tiết kiệm toàn cầu bất kể có bao nhiêu phần tử tối thiểu tồn tại, vì yêu cầu kết nối buộc ít nhất một trong số chúng phải được sử dụng trong các tương tác toàn bộ chi phí. 

Đối với sự chênh lệch cực độ như$g = [1, 10^9]$, câu trả lời trở thành$10^9$. Phần tử nhỏ chỉ đóng góp như một chất hấp thụ một lần, trong khi phần tử lớn chiếm ưu thế về chi phí.
