---
title: "CF 105297J - Acaraj\u00e9"
description: "Chúng ta được cung cấp một danh sách khách hàng tiềm năng, trong đó mỗi khách hàng có một mức giá tối đa mà họ sẵn sàng trả cho một sản phẩm. Nếu chúng ta đặt giá $P$ thì chính xác những khách hàng có $pi ge P$ sẽ mua sản phẩm và mỗi người trong số họ đóng góp $P$ vào doanh thu."
date: "2026-06-23T14:45:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105297
codeforces_index: "J"
codeforces_contest_name: "2024 USP Try-outs"
rating: 0
weight: 105297
solve_time_s: 53
verified: true
draft: false
---

[CF 105297J - Acaraj\u00e9](https://codeforces.com/problemset/problem/105297/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách khách hàng tiềm năng, trong đó mỗi khách hàng có một mức giá tối đa mà họ sẵn sàng trả cho một sản phẩm. Nếu chúng ta đặt giá$P$, thì chính xác những khách hàng đó với$p_i \ge P$sẽ mua sản phẩm và mỗi người trong số họ sẽ đóng góp$P$đến doanh thu. Do đó tổng doanh thu là$P \times \#\{i : p_i \ge P\}$. 

Nhiệm vụ là chọn một mức giá duy nhất$P$từ tập hợp các giá trị thực tế có thể giúp tối đa hóa doanh thu này và đưa ra cả mức giá đã chọn và doanh thu tối đa đạt được. Nếu nhiều mức giá đạt được cùng một doanh thu tối đa thì bất kỳ mức giá nào trong số đó đều có thể được trả lại. 

Hạn chế chính đó là$N$có thể lớn như$10^6$, và mỗi$p_i$có thể đi lên$10^9$. Điều này ngay lập tức loại trừ mọi cách tiếp cận thử mọi mức giá có thể có trong phạm vi số. Ngay cả việc lặp lại tất cả các mức giá đề xuất mà không phân loại cũng sẽ quá chậm, vì việc kiểm tra đơn giản theo từng mức giá sẽ dẫn đến hành vi bậc hai. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều bằng nhau. Ví dụ: nếu tất cả khách hàng có$p_i = 10$, thì bất cứ giá nào$P \le 10$mang lại doanh thu đầy đủ$10 \cdot N$. Một cách tiếp cận bất cẩn chỉ kiểm tra giá từ đầu vào mà không xem xét rằng mức giá tối ưu có thể nhỏ hơn tất cả các yếu tố vẫn có tác dụng ở đây, nhưng chỉ khi nó đánh giá chính xác các ứng viên lặp lại. 

Một vấn đề khác nảy sinh khi mức giá tối ưu không nhất thiết phải là một trong những giá trị cao nhất. Ví dụ, nếu giá cả$[5, 5, 5, 100]$, cài đặt$P = 5$mang lại doanh thu$5 \cdot 4 = 20$, trong khi$P = 100$sản lượng$100 \cdot 1 = 100$. Một giải pháp giả định giá thấp hơn luôn dẫn đến doanh thu cao hơn sẽ thất bại ở đây. 

Cấu trúc gợi ý rằng hàm doanh thu chỉ thay đổi ở các giá trị có trong mảng, vì giữa hai giá trị riêng biệt liên tiếp, số lượng người mua không đổi trong khi giá tăng, điều này chỉ có thể cải thiện doanh thu một cách tuyến tính cho đến điểm dừng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử mọi mức giá có thể có trong số tất cả các số nguyên từ$1$ĐẾN$\max(p_i)$. Với mỗi mức giá ứng viên$P$, chúng tôi đếm có bao nhiêu$p_i \ge P$, sau đó tính doanh thu. Điều này đúng vì nó đánh giá định nghĩa chính xác của vấn đề. 

Tuy nhiên, cách tiếp cận này là không khả thi. Phạm vi giá có thể đạt tới$10^9$và với mỗi mức giá, chúng tôi có thể quét tất cả$N$khách hàng. Điều này dẫn đến$O(N \cdot \max p)$, có giá trị lớn về mặt thiên văn. 

Quan sát quan trọng là số lượng người mua chỉ thay đổi khi giá vượt qua một trong các giá trị trong mảng. Giữa hai giá trị được sắp xếp liên tiếp, số lượng người mua không đổi, do đó hàm doanh thu là tuyến tính theo$P$và đạt cực đại tại một trong các điểm cuối của các khoảng này. Điều này có nghĩa là chúng ta chỉ cần xem xét những mức giá bằng một số$p_i$. 

Khi sắp xếp mảng, chúng ta có thể tính toán một cách hiệu quả số phần tử lớn hơn hoặc bằng mỗi giá trị riêng biệt. Nếu chúng tôi xử lý các giá trị theo thứ tự được sắp xếp, chúng tôi có thể theo dõi số lượng khách hàng còn lại khi chúng tôi tăng ngưỡng giá. 

Bằng cách quét mảng đã sắp xếp và xử lý từng giá trị riêng biệt dưới dạng giá đề xuất, chúng tôi có thể tính toán doanh thu theo thời gian tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot \max p)$|$O(1)$| Quá chậm | 
| Sắp xếp + Quét |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sắp xếp mảng để có thể suy luận về số lượng khách hàng còn lại cho mỗi ngưỡng giá có thể. Việc sắp xếp là cần thiết vì nó chuyển vấn đề sang cách xử lý số lượng hậu tố. 

Sau đó, chúng tôi lặp qua mảng đã sắp xếp, nhưng thay vì đánh giá mọi vị trí một cách độc lập, chúng tôi nhóm các giá trị bằng nhau lại với nhau. Đối với mỗi đề xuất giá riêng biệt$P = p_i$, chúng tôi tính toán có bao nhiêu phần tử lớn hơn hoặc bằng$P$. Nếu mảng được sắp xếp theo thứ tự không giảm và chúng ta đang ở vị trí chỉ mục$i$, thì số lượng người mua là$N - i$. 

Chúng tôi tính toán doanh thu như$P \times (N - i)$và theo dõi mức tối đa trên tất cả các ứng cử viên như vậy. 

Thuật toán tiến hành như sau. 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng$p$theo thứ tự không giảm. Điều này cho phép tất cả khách hàng có ngưỡng cao hơn được nhóm lại với nhau một cách tự nhiên, do đó, độ dài hậu tố thể hiện trực tiếp số lượng người mua. 
2. Khởi tạo một biến để theo dõi doanh thu tốt nhất được tìm thấy cho đến nay và mức giá tương ứng. Chúng tôi bắt đầu với doanh thu bằng 0 và mức giá tùy ý. 
3. Duyệt mảng đã sắp xếp từ trái sang phải. Đối với mỗi chỉ số$i$, đối xử$p[i]$như giá bán ứng cử viên. 
4. Tính số lượng người mua là$N - i$, vì tất cả các phần tử từ$i$ít nhất là đến cuối cùng$p[i]$. Điều này hiệu quả vì việc sắp xếp đảm bảo tính đơn điệu. 
5. Tính doanh thu$R = p[i] \times (N - i)$. So sánh nó với doanh thu tốt nhất cho đến nay và cập nhật nếu nó lớn hơn. Nếu nó bằng nhau, chúng ta có thể giữ một trong hai giá trị. 
6. Tiếp tục quá trình này cho tất cả các chỉ mục, đảm bảo rằng các giá trị trùng lặp không gây ra vấn đề vì chúng mang lại cùng một mức giá nhưng có thể có số lượng hậu tố khác nhau; cả hai đều là những ứng cử viên an toàn. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là với bất kỳ mức giá cố định nào$P$, tập khách hàng mua chính xác là những người có$p_i \ge P$. Giữa hai giá trị riêng biệt liên tiếp trong mảng được sắp xếp, tập hợp này không thay đổi. Tăng dần$P$bên trong một khoảng như vậy chỉ làm giảm doanh thu một cách tuyến tính, do đó giá trị tối ưu không thể nằm hoàn toàn giữa hai giá trị phân biệt liền kề. Do đó, chỉ cần kiểm tra các mức giá bằng các phần tử mảng và tính toán hậu tố sau khi sắp xếp chính xác sẽ nắm bắt được tất cả các trường hợp như vậy là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    best_rev = 0
    best_p = a[0]

    i = 0
    while i < n:
        j = i
        while j < n and a[j] == a[i]:
            j += 1

        p = a[i]
        buyers = n - i
        rev = p * buyers

        if rev > best_rev:
            best_rev = rev
            best_p = p

        i = j

    print(best_p, best_rev)

if __name__ == "__main__":
    solve()
```Bước phân loại là nền tảng của giải pháp. Nó đảm bảo rằng khi chúng ta ở chỉ mục$i$, hậu tố$i \ldots n-1$tương ứng chính xác với tất cả khách hàng có đủ khả năng chi trả$a[i]$. 

Logic nhóm với$j$tránh việc tính toán lại dư thừa cho các giá trị lặp lại. Mặc dù không bắt buộc nghiêm ngặt, nhưng nó giúp lý luận phù hợp với ý tưởng rằng chỉ có những mức giá khác nhau mới quan trọng. 

Việc tính toán doanh thu sử dụng số học an toàn 64-bit hoàn toàn bằng Python, nhưng trong các ngôn ngữ có kiểu số nguyên cố định, bước này cần được cẩn thận vì$10^9 \cdot 10^6$phù hợp với phạm vi có chữ ký 64 bit nhưng không phù hợp với 32 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
15 10 9 10
```Mảng được sắp xếp trở thành:$[9, 10, 10, 15]$| tôi | p[i] | người mua (n - i) | doanh thu | 
| --- | --- | --- | --- | 
| 0 | 9 | 4 | 36 | 
| 1 | 10 | 3 | 30 | 
| 3 | 15 | 1 | 15 | 

Tốt nhất là$P = 9$, doanh thu$36$. 

Điều này cho thấy mặc dù có mức giá cao hơn nhưng việc mất đi số lượng người mua vẫn chiếm ưu thế nhanh chóng. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
```Mảng sắp xếp đã tăng lên. 

| tôi | p[i] | người mua | doanh thu | 
| --- | --- | --- | --- | 
| 0 | 1 | 5 | 5 | 
| 1 | 2 | 4 | 8 | 
| 2 | 3 | 3 | 9 | 
| 3 | 4 | 2 | 8 | 
| 4 | 5 | 1 | 5 | 

Tốt nhất là$P = 3$, doanh thu$9$. 

Điều này thể hiện hành vi không đồng nhất của hàm doanh thu trên các ngưỡng được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian |$O(N)$| Lưu trữ cho mảng đầu vào | 

Các ràng buộc cho phép lên đến$10^6$các phần tử, do đó$O(N \log N)$giải pháp là phù hợp. Việc sử dụng bộ nhớ là tuyến tính và phù hợp thoải mái trong giới hạn điển hình của Python và C++. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like cases
assert run("4\n15 10 9 10\n") == "9 36"
assert run("5\n1 2 3 4 5\n") == "3 9"

# minimum size
assert run("1\n10\n") == "10 10"

# all equal
assert run("5\n7 7 7 7 7\n") == "7 35"

# decreasing order
assert run("4\n100 50 50 1\n") in ["50 150", "100 100"]

# mixed values
assert run("6\n3 3 3 10 10 10\n") == "3 18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 10 10 | trường hợp cơ sở đúng đắn | 
| tất cả đều bình đẳng | 7 35 | xử lý giá trị lặp lại | 
| mảng giảm dần | 50 150 hoặc 100 100 | nhiều ứng viên tối ưu | 
| nhóm hỗn hợp | 3 18 | nhóm logic đúng đắn | 

## Vỏ cạnh 

Đối với đầu vào một phần tử như$N=1$, thuật toán sắp xếp một cách tầm thường và xem xét một ứng cử viên$P = p_1$, tạo ra doanh thu$p_1$. Số lượng hậu tố là 1, vì vậy kết quả là chính xác. 

Đối với trường hợp tất cả các giá trị giống hệt nhau, giả sử$[7,7,7,7,7]$, mọi chỉ số đều tạo ra doanh thu như nhau$7 \cdot 5$. Bước nhóm đảm bảo chúng tôi chỉ đánh giá một lần nhưng ngay cả khi không nhóm, mỗi bản sao đều tạo ra kết quả giống nhau, do đó tính chính xác vẫn được giữ nguyên. 

Đối với trường hợp có giá trị lớn trộn lẫn với nhiều giá trị nhỏ, chẳng hạn như$[1,1,1,100]$, thuật toán đánh giá cả hai$P=1$Và$P=100$. Tại$P=1$, doanh thu là$4$, trong khi ở$P=100$, doanh thu là$100$. Tính toán dựa trên hậu tố nắm bắt chính xác độ tương phản này vì việc sắp xếp đặt giá trị lớn ở cuối, mang lại cho hậu tố kích thước 1, trực tiếp mang lại doanh thu.
