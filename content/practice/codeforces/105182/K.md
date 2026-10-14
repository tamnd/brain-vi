---
title: "CF 105182K - Hoạt động tuần tự"
description: "Chúng ta được cho một chuỗi các số nguyên và chúng ta được phép áp dụng nhiều lần một loại thao tác rất cụ thể: chọn chính xác k vị trí trong mảng và nhân tất cả các phần tử đã chọn với cùng một số nguyên khác 0. Hoạt động này có thể được lặp đi lặp lại bao nhiêu lần."
date: "2026-06-27T04:42:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105182
codeforces_index: "K"
codeforces_contest_name: "The 22nd UESTC Programming Contest - Final"
rating: 0
weight: 105182
solve_time_s: 59
verified: true
draft: false
---

[CF 105182K - Hoạt động trình tự](https://codeforces.com/problemset/problem/105182/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các số nguyên và chúng ta được phép áp dụng nhiều lần một loại thao tác rất cụ thể: chọn chính xác k vị trí trong mảng và nhân tất cả các phần tử đã chọn với cùng một số nguyên khác 0. Hoạt động này có thể được lặp đi lặp lại bao nhiêu lần. 

Mục tiêu là biến đổi chuỗi sao cho tất cả các phần tử trở nên bằng nhau và chúng tôi muốn tìm giá trị tối đa của k mà điều này có thể thực hiện được. 

Một cách hữu ích để diễn giải lại phép toán là nghĩ về cách các số mũ nguyên tố phát triển. Nhân k phần tử đã chọn với cùng một giá trị có nghĩa là chúng ta đang thêm các thay đổi hệ số giống hệt nhau vào chính xác k chỉ số tại một thời điểm. Điều này có nghĩa là “sự khác biệt” giữa các phần tử là cấu trúc thực sự quan trọng chứ không phải giá trị tuyệt đối. 

Ràng buộc n lên tới 10^5 với tổng tổng lên tới 10^6 có nghĩa là về cơ bản chúng ta cần xử lý tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào cố gắng mô phỏng các hoạt động hoặc suy luận theo cặp về các phép biến đổi giữa tất cả các phần tử sẽ thất bại ngay lập tức. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số đều bằng nhau. Trong trường hợp đó, mọi k đều hợp lệ đến n, vì không cần thực hiện thao tác nào. Một trường hợp quan trọng khác là khi các số có hệ số nguyên tố hoàn toàn không liên quan, ví dụ như tất cả các giá trị nguyên tố cùng nhau theo cặp. Trong tình huống đó, chúng ta vẫn luôn có thể làm cho chúng bằng nhau, nhưng k tối đa bị hạn chế bởi các mối quan hệ chia hết về mặt cấu trúc chứ không phải bản thân sự bằng nhau. 

## Phương pháp tiếp cận 

Một cách trực tiếp nhưng không hiệu quả để suy nghĩ về vấn đề là tưởng tượng thực sự đang thực hiện các thao tác. Mỗi thao tác sửa đổi k phần tử theo bước khóa, vì vậy chúng tôi sẽ cố gắng “đồng bộ hóa” dần dần tất cả các giá trị. Tuy nhiên, số lượng các chuỗi thao tác có thể xảy ra là rất lớn và thậm chí việc quyết định xem một k cố định có hoạt động hay không đòi hỏi phải suy luận về các kết hợp cập nhật theo cấp số nhân. 

Sự thay đổi quan trọng là ngừng suy nghĩ về các giá trị và thay vào đó hãy nghĩ về các tỷ lệ. Nếu cuối cùng tất cả các số đều bằng nhau thì đối với hai chỉ số i và j bất kỳ, tỷ lệ a[i] / a[j] phải có thể rút gọn bằng các phép toán được phép. Mỗi phép toán chỉ có thể ảnh hưởng đến k phần tử như nhau, vì vậy điều thực sự quan trọng là có bao nhiêu phần tử chia sẻ sự mất cân bằng số mũ của mỗi thừa số nguyên tố. 

Đối với k cố định, quy trình có thể thành công nếu chúng ta có thể phân phối các điều chỉnh nhiều lần sao cho tất cả sự khác biệt về số mũ có thể được “trung hòa” trong các nhóm có kích thước k. Điều này chỉ có thể thực hiện được khi tổng sự mất cân bằng giữa các số nguyên tố có thể được sắp xếp thành các nhóm có kích thước k, nghĩa là k phải chia tổng “khối lượng sai lệch số mũ” theo một cách rất cụ thể. 

Cách rõ ràng để thấy điều này là sửa giá trị cuối cùng của mục tiêu. Vì tất cả các phần tử đều có kết thúc bằng nhau nên giá trị đó phải được xây dựng từ sự đóng góp trải rộng trên các chỉ số. Đối với mỗi thừa số nguyên tố, chúng tôi xem xét có bao nhiêu phần tử yêu cầu tăng so với giảm số mũ của nó so với mục tiêu. Mỗi thao tác điều chỉnh k chỉ số một cách đồng nhất, do đó nó di chuyển k đơn vị điều chỉnh trên mỗi bước một cách hiệu quả. Điều kiện khả thi chuyển thành ràng buộc chia hết: k phải chia tổng ngân sách điều chỉnh cho tất cả các chỉ số cho mỗi số nguyên tố và ràng buộc chặt chẽ nhất đến từ cấu trúc gcd chung của mảng. 

Quan sát quan trọng là tất cả các hoạt động đều bảo toàn gcd của tỷ lệ giữa các phần tử và mức độ tự do duy nhất nằm ở việc chia tỷ lệ các nhóm được đồng bộ hóa. Điều này dẫn đến thực tế là k tối đa chính xác là ước số lớn nhất sao cho có thể nhóm các chỉ số theo “chữ ký sai phân” bằng nhau, giúp đơn giản hóa kích thước tần số lớn nhất của bất kỳ giá trị nào sau khi chuẩn hóa chuỗi bằng cách chia gcd toàn cục.

Chúng tôi bình thường hóa tất cả các giá trị bằng cách chia cho gcd của toàn bộ mảng. Sau đó, bài toán giảm xuống việc tìm tần số tối đa của bất kỳ giá trị nào trong chuỗi đã chuẩn hóa. Tần số đó là k lớn nhất vì những giá trị giống hệt nhau đó đã không cần điều chỉnh so với nhau và bất kỳ nhóm nào lớn hơn sẽ buộc phải dịch chuyển số mũ không tương thích. 

Do đó, câu trả lời là tần số tối đa của bất kỳ số nào sau khi chia mảng cho gcd của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hoạt động Brute Force | O(exp) | O(n) | Quá chậm | 
| Chuẩn hóa GCD + đếm tần số | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính gcd của toàn bộ mảng. Điều này loại bỏ hệ số chia tỷ lệ chung không ảnh hưởng đến tính khả thi của đẳng thức, vì việc nhân tất cả các phần tử bằng nhau luôn có thể đạt được thông qua các phép toán bao gồm tất cả các vị trí. 
2. Chia mọi phần tử cho gcd này. Sau bước này, tất cả cấu trúc còn lại hoàn toàn mang tính tương đối, nghĩa là mọi sự bình đẳng mà chúng ta đạt được đều phải đến từ việc cân bằng các giá trị đã rút gọn này. 
3. Đếm tần số của từng giá trị chuẩn hóa. 
4. Câu trả lời là tần số tối đa trong số các số đếm này. Điều này tương ứng với nhóm phần tử lớn nhất đã giống hệt nhau, xác định trực tiếp k lớn nhất mà các hoạt động đồng bộ hóa có thể căn chỉnh mọi thứ. 

### Tại sao nó hoạt động 

Sau khi loại bỏ gcd chung, các giá trị còn lại thể hiện sự khác biệt tương đối không thể giảm bớt. Bất kỳ chuỗi hoạt động hợp lệ nào đều bảo toàn các mối quan hệ bình đẳng bên trong các nhóm có chung các giá trị chuẩn hóa giống hệt nhau. Mỗi thao tác ảnh hưởng đến chính xác k chỉ số một cách thống nhất nên chỉ có thể hợp nhất các nhóm có cấu trúc chuẩn hóa bằng nhau mà không tạo ra xung đột trong việc điều chỉnh hệ số. Nhóm lớn nhất như vậy luôn có thể được chọn làm kích thước khối vận hành và bất kỳ k lớn hơn nào cũng sẽ yêu cầu kết hợp các giá trị chuẩn hóa riêng biệt, điều này sẽ buộc các lịch sử nhân không nhất quán không thể giải quyết thành một điểm cuối chung duy nhất. 

## Giải pháp Python```python
import sys
import math
from collections import Counter

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        g = 0
        for x in a:
            g = math.gcd(g, x)
        
        for i in range(n):
            a[i] //= g
        
        freq = Counter(a)
        ans = max(freq.values())
        out.append(str(ans))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tính toán gcd tăng dần, đảm bảo chúng tôi không làm tràn hoặc lưu trữ các sản phẩm trung gian lớn. Sau khi chuẩn hóa, phép chia số nguyên là an toàn vì gcd được đảm bảo chia mọi phần tử. 

Bước đếm tần số sử dụng bản đồ băm, bản đồ này tối ưu cho phạm vi giá trị lớn lên tới 10^9. Cuối cùng, chúng tôi trích xuất tần số tối đa một cách trực tiếp. 

Một điểm tinh tế là chúng ta phải tính toán lại gcd cho mỗi trường hợp thử nghiệm một cách độc lập và chúng ta không được sử dụng lại bộ đếm trong các trường hợp, vì tổng các ràng buộc giữa các thử nghiệm nhưng các giá trị không liên quan. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
6
2 2 3 3 3 6
```Đầu tiên chúng ta tính gcd = 1, do đó mảng không thay đổi. 

| Bước | Mảng | Bản đồ tần số | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu | [2,2,3,3,3,6] | {} | 0 | 
| Đếm | [2,2,3,3,3,6] | {2:2, 3:3, 6:1} | 3 | 

Tần số lớn nhất là 3, tương ứng với giá trị 3. Điều này cho thấy k tốt nhất là 3. 

### Ví dụ 2 

đầu vào:```
1
5
4 4 4 4 8
```gcd = 4, mảng chuẩn hóa trở thành [1,1,1,1,2]. 

| Bước | Mảng | Bản đồ tần số | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu | [1,1,1,1,2] | {} | 0 | 
| Đếm | [1,1,1,1,2] | {1:4, 2:1} | 4 | 

Chúng tôi nhận được câu trả lời 4, nghĩa là nhóm lớn nhất đã được căn chỉnh sau khi chuẩn hóa sẽ xác định k. 

Những dấu vết này cho thấy thuật toán giảm bớt vấn đề để xác định tập hợp con lớn nhất đã nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | tính toán gcd và đếm tần số đều quét mảng một lần | 
| Không gian | O(n) | bản đồ tần số lưu trữ tối đa n giá trị riêng biệt | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 10^6, do đó, giải pháp thời gian tuyến tính phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng an toàn vì mỗi trường hợp thử nghiệm chỉ lưu trữ một bản đồ tần số duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from collections import Counter

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            g = 0
            for x in a:
                g = math.gcd(g, x)
            for i in range(n):
                a[i] //= g
            out.append(str(max(Counter(a).values())))
        return "\n".join(out)

    return solve()

assert run("1\n1\n5\n") == "1", "single element"
assert run("1\n5\n2 2 2 2 2\n") == "5", "all equal"
assert run("1\n5\n1 2 3 4 5\n") == "1", "all distinct"
assert run("1\n6\n2 2 3 3 3 6\n") == "3", "mixed frequencies"
assert run("1\n4\n8 4 2 1\n") == "1", "decreasing chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp n cạnh tối thiểu | 
| tất cả đều bình đẳng | n | k tối đa có thể đạt được | 
| tất cả đều khác biệt | 1 | không thể nhóm | 
| tần số hỗn hợp | 3 | trường hợp điển hình | 
| chuỗi giảm dần | 1 | không trùng lặp sau khi chuẩn hóa | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, gcd bằng chính giá trị đó và quá trình chuẩn hóa sẽ tạo ra một mảng gồm tất cả các phần tử. Bản đồ tần số trả về n, phản ánh chính xác rằng k có thể là n vì bất kỳ thao tác nào cũng bảo toàn đẳng thức ngay lập tức. 

Khi tất cả các phần tử đều khác biệt nhưng có chung một gcd, quá trình chuẩn hóa vẫn có thể tạo ra các bản sao, nhưng chỉ những phần tử trùng lặp đó mới đóng góp vào câu trả lời. Ví dụ: [6,10,14] có gcd 2 và trở thành [3,5,7], cho câu trả lời 1, phù hợp với thực tế là không thể phân nhóm không cần thiết. 

Khi mảng chứa một giá trị lặp lại vượt trội và một vài giá trị ngoại lệ, thuật toán sẽ cô lập một cách tự nhiên cụm giống hệt nhau lớn nhất, vì việc chuẩn hóa gcd không làm xáo trộn các lớp đẳng thức và việc đếm tần số trực tiếp nắm bắt được kích thước nhóm khả thi tối đa.
