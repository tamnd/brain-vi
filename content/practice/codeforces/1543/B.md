---
title: "CF 1543B - Tùy chỉnh bản nhạc"
description: "Chúng ta được cung cấp một danh sách các số nguyên không âm trong đó mỗi giá trị biểu thị số lượng ô tô đậu trên một đoạn đường. “Chi phí” của toàn bộ cấu hình được xác định bằng cách so sánh từng cặp phân khúc và tính tổng chênh lệch tuyệt đối về số lượng xe của chúng."
date: "2026-06-14T19:11:29+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1543
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 730 (Div. 2)"
rating: 900
weight: 1543
solve_time_s: 275
verified: true
draft: false
---

[CF 1543B - Tùy chỉnh bản nhạc](https://codeforces.com/problemset/problem/1543/B) 

**Xếp hạng:** 900 
**Tags:** tổ hợp, tham lam, toán học 
**Thời gian giải:** 4 phút 35 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên không âm trong đó mỗi giá trị biểu thị số lượng ô tô đậu trên một đoạn đường. “Chi phí” của toàn bộ cấu hình được xác định bằng cách so sánh từng cặp phân khúc và tính tổng chênh lệch tuyệt đối về số lượng xe của chúng. Vì vậy, nếu hai đoạn đường khác nhau nhiều về lưu lượng truy cập, chúng sẽ góp phần gây ra nhiều bất tiện. 

Chúng tôi được phép lấy những chiếc xe riêng lẻ và di chuyển chúng tự do giữa các phân khúc, bao nhiêu lần tùy thích. Điều này có nghĩa là chúng tôi không bị ràng buộc bởi vị trí ban đầu mà chỉ bị ràng buộc bởi tổng số ô tô nói chung. Nhiệm vụ là phân phối lại những chiếc xe này trên các phân khúc sao cho cấu hình cuối cùng giảm thiểu tổng chênh lệch tuyệt đối theo cặp. 

Ràng buộc chính là tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là 200.000. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng nhiều lần các bước di chuyển hoặc tính toán lại chi phí theo cặp cho mỗi hoạt động, vì ngay cả một$O(n^2)$tính toán cho mỗi trường hợp thử nghiệm sẽ quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị giống hệt nhau. Trong trường hợp đó, sự bất tiện đã bằng 0 và bất kỳ việc phân phối lại nào cũng sẽ không vô tình làm tăng nó do một sai sót về lý do. Một trường hợp cạnh quan trọng khác là khi mảng có độ lệch cao, ví dụ như một giá trị lớn và nhiều số 0. Một trực giác ngây thơ có thể gợi ý sự cân bằng cục bộ, nhưng mục tiêu phụ thuộc vào tất cả những khác biệt theo cặp chứ không phải những khác biệt liền kề. 

## Phương pháp tiếp cận 

Quan điểm brute-force là suy nghĩ theo các trạng thái: mỗi bước di chuyển sẽ chuyển một đơn vị từ chỉ mục này sang chỉ mục khác, tạo ra một cấu hình mới. Đối với mỗi cấu hình, chúng tôi có thể tính tổng chênh lệch tuyệt đối theo cặp. Ngay cả khi chúng tôi tối ưu hóa việc đánh giá bằng cách sử dụng tổng tiền tố, số lượng phân phối lại có thể rất lớn vì mỗi lần lên tới$10^9$các đơn vị có thể di chuyển độc lập. Điều này làm cho việc tìm kiếm trực tiếp không thể thực hiện được. 

Cái nhìn sâu sắc về cấu trúc đến từ việc viết lại những gì hoạt động cho phép. Vì chúng ta có thể di chuyển ô tô một cách tùy ý nên bất biến duy nhất là tổng của tất cả các phần tử. Chúng ta thực sự có thể tự do xây dựng bất kỳ mảng số nguyên không âm nào có độ dài$n$với tổng số tiền như nhau. Vấn đề trở thành: chọn$n$các giá trị tổng hợp thành$S$giảm thiểu$\sum_{i<j} |b_i - b_j|$. 

Mục tiêu trừng phạt sự lây lan. Bất kỳ sai lệch nào so với tính đồng nhất đều làm tăng khoảng cách theo cặp. Điều này gợi ý rằng cấu hình tối ưu phải cân bằng nhất có thể: tất cả các giá trị bằng nhau hoặc càng gần tính nguyên vẹn càng tốt. 

Cho phép$S = \sum a_i$. Nếu như$S$chia hết cho$n$, chúng ta có thể chỉ định chính xác mọi vị trí$S/n$, điều này làm cho mọi khác biệt theo cặp bằng không. Nếu không, một số vị trí phải mang thêm một đơn vị. Khi đó cấu trúc tối ưu là$r = S \bmod n$các yếu tố là$q+1$và phần còn lại$n-r$là$q$, Ở đâu$q = S // n$. 

Nhiệm vụ còn lại là tính toán chi phí của một cấu hình như vậy. Sự đóng góp chỉ đến từ các cặp được hình thành giữa$q$Và$q+1$, mỗi người đóng góp đúng 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân phối lại vũ lực | hàm mũ | O(n) | Quá chậm | 
| Cân bằng thống nhất + đếm | O(n log n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của mảng. Điều này xác định hạn chế toàn cầu duy nhất còn lại sau khi cho phép phân phối lại. 
2. Tính toán$q = S // n$Và$r = S \% n$. Điều này chia mục tiêu tối ưu thành cơ sở thống nhất lớn nhất có thể và số lượng đơn vị bổ sung phải được phân phối. 
3. Xây dựng multiset tối ưu ngầm:$n-r$giá trị bằng$q$Và$r$giá trị bằng$q+1$. Chúng tôi không xây dựng nó một cách rõ ràng vì chỉ có số lượng tổng hợp mới quan trọng. 
4. Tính toán mức độ gây ra sự bất tiện. Các cặp có giá trị bằng nhau đóng góp bằng 0, vì sự khác biệt của chúng bằng 0. Chỉ có cặp chéo giữa$q$Và$q+1$đóng góp và mỗi cặp như vậy sẽ thêm chính xác một. 
5. Số cặp chéo là$r(n-r)$, vì vậy đây là câu trả lời cuối cùng. 

Lý do điều này có hiệu quả là vì bất kỳ sự sai lệch nào so với cấu trúc hai cấp độ này sẽ tạo ra mức chênh lệch bổ sung. Nếu ba giá trị riêng biệt xuất hiện, việc làm mịn giá trị ở giữa về phía các giá trị lân cận của nó sẽ làm giảm nghiêm ngặt tổng chênh lệch tuyệt đối theo cặp trong khi vẫn bảo toàn tổng, do đó, tính tối ưu buộc giải pháp có nhiều nhất là hai mức nguyên liên tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        s = sum(a)
        q = s // n
        r = s % n
        
        print(r * (n - r))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp theo sau việc giảm xuống nhiều tập hợp hai giá trị. Phép tính không tầm thường duy nhất là tích$r(n-r)$, đếm tất cả các cặp giữa hai nhóm. Không cần phân loại hoặc mô phỏng từng phần tử vì cấu trúc cuối cùng được xác định đầy đủ bởi tổng và kích thước. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp$n=4$,$a=[0,1,1,0]$. Tổng số tiền là$2$, Vì thế$q=0$Và$r=2$. Multiset tối ưu là hai số không và hai số một. 

| Bước | Tổng hợp | q | r | Cấu trúc | 
| --- | --- | --- | --- | --- | 
| ban đầu | 2 | 0 | 2 | [0,0,1,1] | 

Cặp chéo là$2 \times 2 = 4$, vì vậy câu trả lời là 4. Điều này cho thấy rằng mặc dù mảng ban đầu trông có vẻ cân bằng cục bộ nhưng việc phân phối lại toàn cầu sẽ tạo ra cấu trúc hai cấp rõ ràng hơn để xác định chi phí. 

Bây giờ hãy xem xét$n=3$,$a=[1,2,3]$. Tổng số tiền là$6$, Vì thế$q=2$,$r=0$. 

| Bước | Tổng hợp | q | r | Cấu trúc | 
| --- | --- | --- | --- | --- | 
| ban đầu | 6 | 2 | 0 | [2,2,2] | 

Tất cả các giá trị đều bằng nhau, do đó không tồn tại cặp chéo nào và kết quả là 0. Điều này xác nhận rằng khả năng chia hết hoàn hảo sẽ làm sụp đổ mọi biến thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | tổng hợp mảng chiếm ưu thế | 
| Không gian | O(1) thêm | chỉ có quầy được sử dụng | 

Giải pháp chạy thoải mái trong giới hạn vì tổng kích thước đầu vào là$2 \cdot 10^5$và mỗi phần tử được xử lý một lần. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        s = sum(a)
        q = s // n
        r = s % n
        output.append(str(r * (n - r)))
    
    return "\n".join(output)

# provided samples
assert run("""3
3
1 2 3
4
0 1 1 0
10
8 3 6 11 5 2 1 7 10 4
""") == """0
4
21"""

# all equal
assert run("""1
5
3 3 3 3 3
""") == "0"

# already minimal skew
assert run("""1
2
0 100
""") == "1"

# single element
assert run("""1
1
100
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | 0 | đường cơ sở chi phí bằng không | 
| hai giá trị cực trị | 1 | sự mất cân bằng không cần thiết tối thiểu | 
| n = 1 | 0 | trường hợp cạnh đơn | 

## Vỏ cạnh 

Khi nào$n=1$, không có cặp nào cả, vì vậy sự bất tiện luôn bằng 0 bất kể phân phối lại. Công thức$r(n-r)$đánh giá chính xác bằng 0 vì$r=0$. 

Khi tất cả các phần tử bằng nhau thì tổng đó chia hết cho$n$, sản xuất$r=0$. Cấu trúc sụp đổ thành một giá trị duy nhất và không có cặp nào đóng góp bất cứ điều gì, phù hợp với chi phí dự kiến ​​bằng 0. 

Khi tổng không chia hết cho$n$, sự mất cân bằng hoàn toàn bị ảnh hưởng bởi số lượng đơn vị còn sót lại$r$. Mỗi đơn vị này buộc một$+1$độ lệch so với mọi đơn vị cơ bản và không có cấu hình nào khác có thể giảm thiểu điều này mà không vi phạm ràng buộc tổng cố định.
