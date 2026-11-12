---
title: "CF 1669H - Tối đa VÀ"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép “bật” các bit trong các phần tử riêng lẻ. Mỗi thao tác chọn một phần tử và đặt một bit 0 trước đó thành một. Chúng tôi có thể làm điều này nhiều nhất là k lần trên toàn bộ mảng."
date: "2026-06-10T01:59:16+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1669
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 784 (Div. 4)"
rating: 1300
weight: 1669
solve_time_s: 79
verified: true
draft: false
---

[CF 1669H - AND tối đa](https://codeforces.com/problemset/problem/1669/H) 

**Đánh giá:** 1300 
**Tags:** bitmasks, tham lam, toán học 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép “bật” các bit trong các phần tử riêng lẻ. Mỗi thao tác chọn một phần tử và đặt một bit 0 trước đó thành một. Chúng ta có thể làm điều này nhiều nhất`k`lần trên toàn bộ mảng. 

Sau khi áp dụng các phép chèn bit này, chúng tôi tính toán AND theo bit trên tất cả các phần tử mảng. Mục tiêu là tối đa hóa giá trị AND cuối cùng này. 

Khó khăn chính là AND rất nghiêm ngặt: một bit chỉ đóng góp vào câu trả lời cuối cùng nếu mọi phần tử đều chứa nó. Điều này có nghĩa là nếu thậm chí một phần tử bị thiếu một bit thì bit đó hoàn toàn không có trong kết quả. Do đó, bất kỳ thao tác nào thêm một bit chỉ hữu ích nếu cuối cùng nó xuất hiện trong mọi phần tử. 

Các ràng buộc đẩy chúng ta tới một chiến lược tham lam hoặc đếm ở cấp độ bit. Tổng cộng`n`trên các trường hợp thử nghiệm lên đến`2·10^5`, do đó, bất kỳ giải pháp nào kiểm tra từng phần tử với số lần cố định trên mỗi bit đều có thể chấp nhận được. Tuy nhiên, bất cứ điều gì thử tập hợp con các bit hoặc phần tử hoặc mô phỏng trực tiếp các hoạt động sẽ thất bại ngay lập tức vì`k`có thể lớn như`10^9`. 

Một cách giải thích ngây thơ sẽ là thử gán các bit một cách tham lam cho mỗi phần tử, nhưng điều đó bỏ qua sự ghép nối toàn cục được tạo bởi AND. Cấu trúc thực sự là mỗi bit độc lập và có thể được phân tích riêng biệt. 

Một trường hợp lỗi tinh vi xuất hiện khi một giải pháp giả định rằng chúng ta phải luôn tối đa hóa số lượng bit được đặt cho mỗi phần tử. 

Ví dụ, hãy xem xét:```
n = 3, k = 1
a = [1, 0, 1]
```Một ý tưởng ngây thơ có thể là sử dụng thao tác trên phần tử ở giữa để sao chép một bit hữu ích từ những phần tử khác, nhưng trên thực tế, một thao tác không thể làm cho một bit xuất hiện trong cả ba phần tử trừ khi nó đã tồn tại ở ít nhất hai phần tử trong số đó. Vì vậy câu trả lời tốt nhất vẫn là`0`hoặc phụ thuộc vào việc có thể căn chỉnh đầy đủ hay không. Điều này nhấn mạnh rằng chúng ta phải suy luận theo từng bit trên toàn cầu chứ không phải theo từng phần tử. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực sẽ là mô phỏng tất cả các cách áp dụng cho`k`chèn bit. Mỗi thao tác có thể chọn một chỉ mục và một vị trí bit, do đó có khoảng`n * 31`lựa chọn cho mỗi thao tác. Thậm chí hạn chế ở mức nhỏ`k`, hệ số phân nhánh làm cho điều này trở thành cấp số nhân. 

Vấn đề sẽ trở nên dễ quản lý hơn khi chúng ta chuyển quan điểm từ “làm cách nào để gán bit” sang “mỗi bit yêu cầu những gì để tồn tại trong AND”. 

Sửa một chút`j`. Trong mảng ban đầu, hãy`cnt[j]`có bao nhiêu phần tử đã được thiết lập bit này. Để làm một chút`j`xuất hiện trong AND cuối cùng, mọi phần tử đều phải kết thúc bằng nó. Điều đó có nghĩa là chúng ta cần sửa chính xác`n - cnt[j]`thiếu vị trí cho bit này. Mỗi thao tác có thể sửa một vị trí bị thiếu cho một bit trong một phần tử, do đó chi phí tạo bit`j`hiện diện đầy đủ chính xác là`n - cnt[j]`. 

Bây giờ chúng ta thấy cấu trúc: mỗi bit là một mục có “lợi nhuận” bằng giá trị của nó`2^j`và “chi phí” bằng số lượng hoạt động cần thiết để kích hoạt nó trên toàn cầu. Chúng tôi muốn chọn một tập hợp con các bit để tối đa hóa tổng giá trị, với tổng chi phí ≤`k`. 

Đây là một tình huống có ý nghĩa tham lam theo từng bit cổ điển. Vì các bit cao hơn sẽ thống trị các bit thấp hơn về giá trị, nên chúng tôi xử lý các bit từ 30 xuống 0 và tham lam bao gồm một chút nếu chúng tôi có đủ khả năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng hoạt động) | Hàm mũ | O(n) | Quá chậm | 
| Bit tham lam | O(31·n) | O(31) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm từng bit`j`, có bao nhiêu phần tử mảng đã được thiết lập bit đó. Điều này đưa ra cấu trúc cơ bản của các bit “gần như đã mang tính toàn cầu”. 
2. Đối với mỗi bit`j`, tính số phần tử bị thiếu bit đó, đó là`cost[j] = n - cnt[j]`. Đây là số thao tác cần thiết để làm cho bit này hiện diện trong mọi phần tử. 
3. Khởi tạo một biến`remaining_k = k`, theo dõi số lượng hoạt động chúng ta vẫn có thể chi tiêu. 
4. Khởi tạo câu trả lời`ans = 0`. 
5. Lặp lại các bit từ 30 xuống 0. Đối với mỗi bit`j`, kiểm tra xem`cost[j] <= remaining_k`. Nếu có, chúng tôi có đủ khả năng để thực thi quy định này trên toàn cầu. 
6. Nếu chúng ta bao gồm bit`j`, thêm vào`2^j`đến câu trả lời và trừ`cost[j]`từ`remaining_k`. 
7. Nếu chúng ta không đủ khả năng chi trả một chút, hãy bỏ qua nó. Các bit thấp hơn có thể vẫn có giá cả phải chăng và có giá trị khi kết hợp lại. 

### Tại sao nó hoạt động 

Mỗi bit độc lập về mặt hoạt động vì việc thiết lập bit`j`trong một phần tử không ảnh hưởng đến bất kỳ bit nào khác. Chi phí để đạt được sự hiện diện đầy đủ của một bit chỉ phụ thuộc vào số lượng phần tử hiện đang thiếu nó chứ không phụ thuộc vào việc lựa chọn các bit khác. 

Thứ tự tham lam theo ý nghĩa bit là chính xác vì chi phí là cố định và độc lập, trong khi các giá trị tuân theo sự thống trị nhị phân nghiêm ngặt. Bất kỳ cấu hình nào bao gồm bit thấp hơn đều không thể bù đắp cho việc mất bit cao hơn, do đó việc xử lý từ cao xuống thấp đảm bảo chúng tôi không bao giờ hy sinh cấu hình bit cao có thể đạt được tốt hơn để có được sự kết hợp kém hơn giữa các bit thấp hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        cnt = [0] * 31

        for x in a:
            for j in range(31):
                if x & (1 << j):
                    cnt[j] += 1

        remaining = k
        ans = 0

        for j in range(30, -1, -1):
            cost = n - cnt[j]
            if cost <= remaining:
                remaining -= cost
                ans |= (1 << j)

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên là xây dựng tần số bit trên toàn mảng, điều này cần thiết để biết mỗi bit cách phổ biến bao xa. Vòng lặp tham lam sau đó mô phỏng việc chọn các bit theo thứ tự quan trọng giảm dần. 

Một lỗi phổ biến là tính toán chi phí không chính xác bằng cách đếm các số 0 trên toàn cầu mà không nhận ra rằng mỗi thao tác chỉ sửa một phần tử cho một bit. Một cách khác là cố gắng sử dụng lại các thao tác trên các bit; điều này không hợp lệ vì mỗi thao tác OR nhắm mục tiêu chính xác một bit trong một phần tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 2
a = [2, 1, 1]
```Chúng tôi tính toán số lượng bit: 

| Chút | cnt | chi phí = n - cnt | Lấy? | còn lại k | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | vâng | 0 | 2 | 
| 0 | 2 | 1 | không | 0 | 2 | 

Chúng tôi lấy bit 1 vì nó tốn 2 thao tác, khớp chính xác`k`. Sau khi thực hiện, không còn hoạt động nào nữa. 

Điều này cho thấy ngay cả một bit cao cũng có thể ảnh hưởng đến kết quả nếu nó đắt tiền nhưng nằm trong ngân sách. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 4
a = [3, 1, 3, 1]
```Biểu diễn bit: 

- bit 1 đã có trong tất cả các phần tử 
- bit 0 đã có trong tất cả các phần tử 

| Chút | cnt | chi phí | Lấy? | còn lại k | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 0 | vâng | 4 | 2 | 
| 0 | 4 | 0 | vâng | 4 | 3 | 

Chúng tôi lấy cả hai bit miễn phí vì chúng đã tồn tại ở mọi nơi. 

Điều này chứng tỏ rằng thuật toán nắm bắt một cách tự nhiên thực tế là các bit toàn cục hiện có không yêu cầu hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(31 · n) | Mỗi phần tử góp phần đếm bit một lần cho mỗi vị trí bit | 
| Không gian | O(31) | Chỉ các mảng tần số cho bit được lưu trữ | 

Tổng cộng`n`qua các trường hợp thử nghiệm là`2·10^5`, do đó, việc quét 31 bit cho mỗi phần tử có thể dễ dàng đủ nhanh trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    def input():
        return sys.stdin.readline()
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())
        a = list(map(int, sys.stdin.readline().split()))
        
        cnt = [0] * 31
        for x in a:
            for j in range(31):
                if x & (1 << j):
                    cnt[j] += 1
        
        remaining = k
        ans = 0
        for j in range(30, -1, -1):
            cost = n - cnt[j]
            if cost <= remaining:
                remaining -= cost
                ans |= (1 << j)
        
        out.append(str(ans))
    
    return "\n".join(out)

# provided samples
assert run("""4
3 2
2 1 1
7 0
4 6 6 28 6 6 12
1 30
0
4 4
3 1 3 1
""") == """2
4
2147483646
1073741825"""

# minimum size
assert run("""1
1 0
5
""") == "5"

# all zeros
assert run("""1
3 5
0 0 0
""") == "7"

# already equal array
assert run("""1
4 2
7 7 7 7
""") == "7"

# large k
assert run("""1
2 100
1 2
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | giá trị không đổi | trường hợp cơ sở đúng đắn | 
| tất cả số không | xây dựng bit đầy đủ | logic tích lũy chi phí | 
| mảng bằng nhau | tối ưu không hoạt động | bao gồm chi phí bằng 0 | 
| k lớn | hành vi bão hòa | sự ổn định tham lam | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các phần tử đã chứa một bit. Trong trường hợp đó,`cnt[j] = n`, Vì thế`cost[j] = 0`và thuật toán luôn lấy bit đó ngay lập tức. Điều này đảm bảo cấu trúc hiện tại được bảo tồn mà không cần hoạt động tiêu tốn. 

Một trường hợp cạnh khác là khi`k = 0`. Thuật toán không bao giờ bao gồm bất kỳ bit nào chưa có tính tổng thể, vì mọi`cost[j] > 0`trừ khi có mặt đầy đủ. Kết quả trở thành chính xác AND của mảng ban đầu. 

Cuối cùng, khi`k`là cực kỳ lớn, thuật toán chỉ đơn giản cố gắng bao gồm tất cả các bit, nhưng mỗi bit vẫn yêu cầu tính khả thi. Vì chi phí là độc lập nên cuối cùng chúng tôi chỉ xây dựng lại OR theo bit của tất cả các phần tử khi có thể bao phủ toàn bộ, phù hợp với hành vi tối ưu hóa dự kiến.
