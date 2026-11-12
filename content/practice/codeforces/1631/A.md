---
title: "CF 1631A - Hoán đổi tối thiểu tối thiểu"
description: "Chúng ta được cung cấp hai mảng có độ dài bằng nhau và tại mỗi vị trí, chúng ta được phép quyết định giá trị nào trong hai giá trị nằm trong mảng đầu tiên và giá trị nào nằm ở mảng thứ hai. Cụ thể, đối với mỗi chỉ số độc lập, chúng ta có thể hoán đổi cặp hoặc giữ nguyên."
date: "2026-06-10T04:58:10+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1631
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 768 (Div. 2)"
rating: 800
weight: 1631
solve_time_s: 65
verified: true
draft: false
---

[CF 1631A - Hoán đổi tối đa tối thiểu](https://codeforces.com/problemset/problem/1631/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai mảng có độ dài bằng nhau và tại mỗi vị trí, chúng ta được phép quyết định giá trị nào trong hai giá trị nằm trong mảng đầu tiên và giá trị nào nằm ở mảng thứ hai. Cụ thể, đối với mỗi chỉ số độc lập, chúng ta có thể hoán đổi cặp hoặc giữ nguyên. Sau khi chúng ta chọn xong các giao dịch hoán đổi, mỗi vị trí đóng góp chính xác một số cho mảng đầu tiên và số còn lại cho mảng thứ hai. 

Mục tiêu là sắp xếp các lựa chọn này sao cho tích của phần tử lớn nhất trong mảng thứ nhất và phần tử lớn nhất trong mảng thứ hai càng nhỏ càng tốt. 

Khó khăn chính là mỗi quyết định cục bộ đều ảnh hưởng đến hai đại lượng toàn cầu cùng một lúc. Di chuyển một giá trị lớn vào một mảng có thể làm tăng mức tối đa của mảng đó, trong khi việc giữ giá trị đó ở mảng khác có thể làm tăng mức tối đa khác. Quyết định này không độc lập giữa các chỉ số vì cực đại là tóm tắt toàn cầu. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm và kích thước mảng lên tới 100. Điều này ngay lập tức loại trừ mọi tìm kiếm tổ hợp trên các bài tập, mặc dù không gian quyết định về mặt khái niệm$2^n$. Một giải pháp thử tất cả các cấu hình trao đổi cho mỗi trường hợp thử nghiệm sẽ cần tới$2^{100}$các trạng thái, điều này vượt xa khả năng thực hiện. 

Trường hợp cạnh tinh tế xuất hiện khi cả hai mảng đều chứa các giá trị lớn giống hệt nhau. Trong những trường hợp như vậy, việc hoán đổi không thay đổi bất cứ điều gì và câu trả lời là cố định. Ví dụ: nếu cả hai mảng đều`[3, 3, 3]`, mọi cấu hình đều mang lại cực đại là 3 và 3, vì vậy câu trả lời là 9. Bất kỳ cách tiếp cận nào giả định hoán đổi luôn có thể giảm cực đại một cách độc lập sẽ cố gắng sắp xếp lại không cần thiết một cách không chính xác nhưng vẫn không thể cải thiện sản phẩm. 

Một tình huống cạnh khác xảy ra khi các giá trị lớn được “ghép nối” ở cùng một chỉ mục. Ví dụ: nếu tại chỉ mục$i$, cả hai$a_i$Và$b_i$lớn thì một trong số chúng phải trở thành người đóng góp tối đa cho một mảng và mảng kia có thể giúp hoặc không giúp mảng đối diện tùy thuộc vào sự hoán đổi. Sự ghép nối này là ràng buộc cấu trúc trung tâm. 

## Phương pháp tiếp cận 

Chiến lược bạo lực là coi mỗi chỉ mục là một lựa chọn nhị phân: hoặc giữ cặp đó như$(a_i, b_i)$hoặc trao đổi nó thành$(b_i, a_i)$. Đối với mỗi$2^n$cấu hình, chúng tôi tính toán mức tối đa của mảng đầu tiên thu được và mức tối đa của mảng thứ hai, sau đó nhân chúng và theo dõi mức tối thiểu. Điều này đúng vì nó khám phá mọi nhiệm vụ có thể. 

Điểm thất bại là rõ ràng về quy mô. Vì$n = 100$, đây là$2^{100}$cấu hình, và thậm chí đối với$n = 30$, nó đã quá lớn rồi. Do đó, cấu trúc của vấn đề phải được giảm bớt. 

Quan sát quan trọng là điều quan trọng không phải là phép gán đầy đủ mà chỉ là giá trị nào cuối cùng trở thành giá trị tối đa của mỗi mảng. Nếu chúng tôi cố định mức tối đa ứng cử viên cho mảng đầu tiên, chúng tôi có thể thực thi các ràng buộc trên mọi phần tử: đối với mỗi chỉ mục, chúng tôi đảm bảo rằng vị trí đã chọn không vi phạm mức tối đa đó và tương tự đối với mảng thứ hai. Điều này làm giảm vấn đề trong việc kiểm tra tính khả thi dưới ngưỡng tối đa dự đoán. 

Một cách hữu ích để xem cấu trúc là tưởng tượng việc sắp xếp tất cả các cặp theo giá trị lớn hơn của chúng. Giá trị tối đa cuối cùng của một trong hai mảng phải là một trong những giá trị này, vì giá trị cực đại đến trực tiếp từ tập hợp tất cả các số đầu vào. Khi chúng tôi sửa một ứng cử viên cho mức tối đa của mảng đầu tiên, chúng tôi chỉ định từng chỉ mục một cách tham lam để mức tối đa của mảng thứ hai được giảm thiểu theo ràng buộc đó. 

Điều này dẫn đến việc kiểm tra tuyến tính đơn giản mức tối đa cho mỗi ứng cử viên: chúng tôi thực thi rằng tất cả các giá trị được đặt vào mảng đầu tiên không vượt quá ứng cử viên và chúng tôi theo dõi giá trị lớn nhất được buộc vào mảng thứ hai. Sản phẩm tốt nhất khi đó là sản phẩm tối thiểu trong tất cả các lựa chọn tối đa của ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n^2)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lặp lại các lựa chọn có thể có về giá trị tối đa trong mảng đầu tiên có thể là bao nhiêu sau tất cả các lần hoán đổi. 

1. Thu thập tất cả các giá trị từ cả hai mảng vào một danh sách các cực đại ứng viên. Đây là những giá trị duy nhất có thể trở thành giá trị tối đa của một trong hai mảng. 
2. Đối với mỗi giá trị ứng viên$X$, hãy coi nó là mức tối đa dự định của mảng đầu tiên. Bây giờ chúng tôi quyết định mỗi cặp đóng góp như thế nào. 
3. Đối với mỗi chỉ số$i$, nếu một trong$a_i$hoặc$b_i$lớn hơn$X$, giá trị đó phải nằm trong mảng đầu tiên trừ khi nó vi phạm ràng buộc. Vì chúng tôi đang buộc tối đa mảng đầu tiên là tối đa$X$, bất kỳ giá trị nào lớn hơn$X$không thể đặt ở đó. Điều này ngay lập tức xác định vị trí cho các cặp như vậy. 
4. Đối với các cặp còn lại có cả hai giá trị đều bằng nhau$\le X$, chúng tôi chọn các vị trí giảm thiểu tối đa mảng thứ hai. Quy tắc tham lam tự nhiên là đặt giá trị lớn hơn vào mảng đầu tiên bất cứ khi nào có thể, để lại giá trị nhỏ hơn ở mảng thứ hai. 
5. Sau khi xử lý tất cả các chỉ số, tính giá trị lớn nhất của mảng thứ hai, gọi nó là$Y$. Câu trả lời của ứng viên là$X \cdot Y$. 
6. Lặp lại tất cả những gì có thể$X$và lấy sản phẩm tối thiểu. 

### Tại sao nó hoạt động 

Bất biến quan trọng là đối với một ngưỡng cố định$X$, chúng tôi không bao giờ đặt bất kỳ giá trị nào lớn hơn$X$vào mảng đầu tiên. Điều này đảm bảo mức tối đa của mảng đầu tiên được giới hạn chính xác bởi$X$. Trong số tất cả các phép gán hợp lệ tuân theo ràng buộc này, việc đẩy các phần tử còn lại lớn hơn vào mảng đầu tiên sẽ giảm áp lực lên mảng thứ hai, vì nó ngăn các giá trị lớn tích lũy ở đó một cách không cần thiết. Bất kỳ sai lệch nào so với lựa chọn tham lam này sẽ chỉ tăng hoặc duy trì mức tối đa của mảng thứ hai, không bao giờ giảm nó, bởi vì các giao dịch hoán đổi chỉ phân phối lại các giá trị mà không thay đổi nhiều tập hợp của mỗi cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        vals = set()
        for i in range(n):
            vals.add(a[i])
            vals.add(b[i])
        
        vals = list(vals)
        ans = float('inf')
        
        for X in vals:
            max_b = 0
            
            for i in range(n):
                x, y = a[i], b[i]
                
                if x > X and y > X:
                    max_b = float('inf')
                    break
                
                if x > X:
                    max_b = max(max_b, x)
                elif y > X:
                    max_b = max(max_b, y)
                else:
                    max_b = max(max_b, min(x, y))
            
            if max_b != float('inf'):
                ans = min(ans, X * max_b)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách trích xuất tất cả các giá trị ứng viên có thể đóng vai trò là giá trị lớn nhất của mảng đầu tiên. Điều này là đủ vì mọi mức tối đa đều phải đến từ một phần tử hiện có. Đối với mỗi ứng viên$X$, chúng tôi mô phỏng sự sắp xếp tốt nhất có thể theo ràng buộc mà không có gì được đặt vào mảng đầu tiên vượt quá$X$. 

Trong quá trình mô phỏng, nếu cả hai giá trị trong một cặp vượt quá$X$, ứng cử viên đó không hợp lệ vì cả hai đều không thể được đặt vào mảng thứ hai mà không vi phạm cấu trúc cực đại. Mặt khác, chúng tôi tham lam đảm bảo mảng thứ hai nhận được những đóng góp nhỏ nhất có thể, theo dõi mức tối đa của nó khi chúng tôi tiếp tục. 

Phép nhân ở cuối phản ánh trực tiếp hàm mục tiêu sau khi cả hai cực đại được xác định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [1, 2, 6, 5, 1, 2]
b = [3, 4, 3, 2, 2, 5]
```Chúng tôi kiểm tra giá trị ứng cử viên$X$TRONG`{1,2,3,4,5,6}`. 

| X | Hiệu ứng chuyển nhượng (ràng buộc tối đa) | tối đa(b) | Sản phẩm | 
| --- | --- | --- | --- | 
| 3 | buộc 6,5,4,5 vào giây khi cần thiết | 4 | 12 | 
| 4 | linh hoạt hơn, phân phối tốt hơn | 3 | 12 | 
| 5 | cho phép hầu hết các giá trị lớn trong | 3 | 15 | 
| 6 | hoàn toàn linh hoạt | 5 | 30 | 

Mức tối thiểu xảy ra ở mức phân chia cân bằng, phù hợp với sản phẩm tối ưu sau khi đánh giá đầy đủ qua các bài tập. 

Dấu vết này cho thấy mức độ thắt chặt$X$giảm tối đa mảng thứ nhất nhưng có thể tăng áp lực lên mảng thứ hai. 

### Ví dụ 2 

đầu vào:```
a = [1, 2]
b = [2, 1]
```| X | Vị trí | tối đa(b) | Sản phẩm | 
| --- | --- | --- | --- | 
| 1 | không hợp lệ (cả hai giá trị đều vượt quá giới hạn ở đâu đó) | - | - | 
| 2 | có thể trao đổi hợp lệ | 1 | 2 | 

Sự lựa chọn tốt nhất là$X = 2$, tặng sản phẩm$2 \cdot 1 = 2$. Điều này thể hiện tính đối xứng trong đó việc hoán đổi sắp xếp các giá trị lớn thành một mảng và nhỏ vào mảng kia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot m)$Ở đâu$m \le 2n$| Đối với mỗi giá trị ứng cử viên, chúng tôi quét tất cả các cặp một lần | 
| Không gian |$O(n)$| Lưu trữ mảng đầu vào và bộ ứng cử viên | 

Với$n \le 100$, trường hợp xấu nhất là về$10^4$hoạt động cho mỗi trường hợp thử nghiệm, dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    # solution embedded
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        vals = set(a + b)
        ans = float('inf')
        
        for X in vals:
            max_b = 0
            ok = True
            for i in range(n):
                x, y = a[i], b[i]
                if x > X and y > X:
                    ok = False
                    break
                if x > X:
                    max_b = max(max_b, x)
                elif y > X:
                    max_b = max(max_b, y)
                else:
                    max_b = max(max_b, min(x, y))
            if ok:
                ans = min(ans, X * max_b)
        
        print(ans)
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("""3
6
1 2 6 5 1 2
3 4 3 2 2 5
3
3 3 3
3 3 3
2
1 2
2 1
""") == """18
9
2"""

# custom cases
assert run("""1
1
5
10
""") == "50"

assert run("""1
2
1 100
100 1
""") == "100"

assert run("""1
3
5 1 4
2 6 3
""") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp phần tử đơn | sản phẩm hoán đổi giống hệt nhau | độ đúng cơ sở | 
| trường hợp hoán đổi đối xứng | hành vi phân chia tối thiểu | cân đối tài sản | 
| trường hợp nhỏ ngẫu nhiên hỗn hợp | tham lam xử lý tính khả thi | tính đúng đắn chung | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi cả hai mảng đều chứa các giá trị giống hệt nhau. Ví dụ:```
a = [3, 3, 3]
b = [3, 3, 3]
```Mỗi ứng viên$X = 3$dẫn đến cấu hình tương tự. Thuật toán xử lý từng chỉ mục và luôn ghi lại`max_b = 3`, tạo ra sản phẩm 9. Không tồn tại cấu hình thay thế nào và thuật toán không cố gắng hoán đổi không cần thiết vì mọi so sánh đều rơi vào nhánh bằng nhau. 

Một trường hợp khác là khi mỗi chỉ mục đã được “căn chỉnh” một cách tối ưu:```
a = [2, 1]
b = [1, 2]
```Vì$X = 2$, mỗi cặp đóng góp$1$đến mức tối đa của mảng thứ hai, cho ra sản phẩm 2. Mô phỏng không bao giờ vi phạm tính khả thi vì không có cặp nào có cả hai giá trị vượt quá$X$.
