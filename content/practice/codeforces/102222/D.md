---
title: "CF 102222D - Hãy ngồi vào chỗ của bạn"
description: "Có hai câu hỏi xác suất liên quan. Trong câu hỏi đầu tiên, hành khách từ 1 đến (n) lên xe theo thứ tự tăng dần. Hành khách 1 đã mất thông tin về chỗ ngồi được chỉ định của mình và chọn ngẫu nhiên một trong (n) chỗ ngồi giống nhau."
date: "2026-08-17T22:03:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102222
codeforces_index: "D"
codeforces_contest_name: "2018-2019 ACM-ICPC, China Multi-Provincial Collegiate Programming Contest"
rating: 0
weight: 102222
solve_time_s: 101
verified: true
draft: false
---

[CF 102222D - Hãy ngồi vào chỗ](https://codeforces.com/problemset/problem/102222/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có hai câu hỏi xác suất liên quan. 

Trong câu hỏi đầu tiên, hành khách từ 1 đến (n) lên xe theo thứ tự tăng dần. Hành khách 1 đã mất thông tin về chỗ ngồi được chỉ định của mình và chọn ngẫu nhiên một trong (n) chỗ ngồi giống nhau. Mọi hành khách sau sẽ ngồi vào chỗ được chỉ định nếu chỗ đó vẫn còn trống. Nếu có người ngồi thì hành khách đó cũng chọn thống nhất những ghế hiện đang trống. Chúng ta cần xác suất để hành khách (n), hành khách cuối cùng lên máy bay, ngồi vào ghế (n). 

Trong câu hỏi thứ hai, có (m) hành khách và (m) chỗ ngồi, nhưng bây giờ bản thân thứ tự lên máy bay là một hoán vị ngẫu nhiên thống nhất. Hành khách 1 vẫn không biết chỗ ngồi được chỉ định của mình và quy tắc tương tự được sử dụng bất cứ khi nào hành khách phát hiện ra rằng chỗ ngồi được chỉ định của họ đã có người ngồi. Chúng ta cần xác suất để người lên cuối cùng có được chỗ ngồi riêng. 

Mỗi trường hợp thử nghiệm chứa (n) và (m), cả hai đều có nhiều nhất là 50. Có nhiều nhất là 50 trường hợp thử nghiệm. Các giới hạn số nhỏ có thể gợi ý quy hoạch động, nhưng lời giải thực tế lại đơn giản hơn nhiều. Chúng ta có thể rút gọn cả hai câu trả lời về dạng đóng, do đó tổng công việc là không đổi cho mỗi trường hợp kiểm thử. Tuy nhiên, giới hạn 50 vẫn hữu ích vì nó cho phép chúng ta xác minh các công thức với độ lặp lại nhỏ nếu muốn, đồng thời tránh mọi nhu cầu về máy móc số lớn. 

Trường hợp cạnh đầu tiên là (n=1). Chỉ có hành khách 1 và chỉ có ghế 1 nên hành khách phải ngồi đúng ghế. Đối với đầu vào`1 1`, đầu ra là`Case #1: 1.000000 1.000000`. Việc triển khai bất cẩn áp dụng một cách mù quáng câu trả lời (1/2) thông thường cho vấn đề đầu tiên sẽ in sai 0,5. 

Trường hợp cạnh thứ hai là (n=2). Hành khách 1 chọn một trong hai chỗ với xác suất (1/2). Nếu anh ta chọn ghế 1 thì hành khách 2 đúng; nếu chọn ghế 2 thì hành khách 2 buộc phải ngồi ghế 1. Như vậy đáp án đầu tiên đúng là 0,5. Đối với đầu vào`2 2`, đầu ra là`Case #1: 0.500000 0.750000`. Áp dụng công thức thứ hai cho câu hỏi đầu tiên sẽ cho cùng một giá trị ở đây, nhưng đó là sự trùng hợp ngẫu nhiên, không phải là một suy luận hợp lệ. 

Trường hợp cạnh thứ ba là (m=1). Trong phiên bản theo thứ tự ngẫu nhiên, hành khách 1 nhất thiết phải là hành khách cuối cùng và chỉ có một chỗ ngồi duy nhất, vì vậy câu trả lời thứ hai là 1. Đối với đầu vào`1 1`, cả hai câu trả lời phải chính xác là 1. Công thức ((m+1)/(2m)) cũng cho kết quả 1, do đó không cần nhánh đặc biệt nào cho bài toán thứ hai. 

Trường hợp ranh giới trong đó hai giá trị bằng nhau cũng hữu ích. Đối với đầu vào`50 50`, câu trả lời đầu tiên là 0,5 vì (n>1), trong khi câu trả lời thứ hai là (51/100=0,51). Việc triển khai vô tình sử dụng (n) thay vì (m) trong công thức thứ hai sẽ không thành công trong trường hợp này. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp về mặt khái niệm là đơn giản. Chúng ta có thể liệt kê mọi thứ tự lên máy bay, liệt kê mọi lựa chọn chỗ ngồi ngẫu nhiên có thể có, mô phỏng hành khách và đếm xem có bao nhiêu kết quả khiến hành khách cuối cùng ngồi đúng ghế. Điều này đúng vì mọi lựa chọn ngẫu nhiên cơ bản và mọi thứ tự lên máy bay đều có thể được biểu diễn một cách rõ ràng. 

Vấn đề là số lượng kết quả. Đối với phiên bản thứ tự ngẫu nhiên, có thể có (m!) hoán vị lên máy bay. Trong quá trình mô phỏng, có thể có tới (m-1) lựa chọn chỗ ngồi ngẫu nhiên và mỗi lựa chọn như vậy có nhiều nhất (m) kết quả có thể xảy ra. Do đó, giới hạn trên đơn giản là (m! \cdot m^{m-1}) nhánh kết quả hoàn chỉnh, với hệ số khác là (O(m)) nếu mỗi nhánh được mô phỏng trực tiếp. Với (m=50), điều này vượt xa mọi khả năng có thể thực hiện được. Ngay cả một lực lượng vũ phu dựa trên nhà nước cẩn thận hơn nhiều cũng sẽ là công việc không cần thiết. 

Lực lượng vũ phu hoạt động vì bản thân quá trình này dễ mô phỏng nhưng không thành công vì nó coi mọi hành khách và mọi lựa chọn ngẫu nhiên là chi tiết độc lập. Nhận xét quan trọng là hầu hết mọi người hoàn toàn không bị ảnh hưởng bởi chiếc vé bị mất. Khi hành khách 1 ngồi vào ghế của hành khách khác, tất cả hành khách trước hành khách bị ảnh hưởng đó vẫn ngồi đúng. Sự không chắc chắn duy nhất chuyển sang hành khách bị mất chỗ ngồi. Điều này thu gọn quá trình thành một bản sao nhỏ hơn của cùng một vấn đề xác suất. 

Đối với câu hỏi đầu tiên, gọi (f(k)) là xác suất hành khách cuối cùng đúng khi có (k) hành khách và hành khách 1 là người duy nhất không có thông tin về chỗ ngồi của họ. Hành khách 1 có xác suất (1/k) chọn ghế 1, điều này ngay lập tức khiến những người khác đúng. Nếu hành khách 1 chọn chỗ ngồi (j>1), hành khách từ 2 đến (j-1) không bị ảnh hưởng và hành khách (j) trở thành hành khách bị di dời mới. Từ thời điểm đó trở đi, phần không chắc chắn chính xác là vấn đề tương tự đối với hành khách (j,j+1,\ldots,k). 

Điều này gây ra sự tái phát 

[ 
f(k)=\frac{1}{k}\left(1+\sum_{j=2}^{k} f(k-j+1)\right) 
=\frac{1}{k}\sum_{r=1}^{k-1}f(r). 
] 

Với (f(1)=1), phép truy toán cho kết quả (f(2)=1/2). Khi (f(2)=f(3)=\cdots=f(k-1)=1/2), chúng ta nhận được 

[ 
f(k)=\frac{(k-1)/2}{k}=\frac{k-1}{2k}. 
] 

Chỉ riêng biểu thức đó dường như đã mâu thuẫn với câu trả lời đã biết, bởi vì sự tái diễn phải bao gồm sự kiện trong đó hành khách 1 chiếm chỗ của chính họ với tư cách là một nhánh thành công hoàn toàn. Viết phép truy toán cẩn thận với bài toán rút gọn sẽ cho 

[ 
f(k)=\frac{1}{k}+\frac{1}{k}\sum_{r=1}^{k-1}f(r). 
] 

Bây giờ (f(1)=1), (f(2)=1/2), và nếu tất cả các giá trị từ 2 đến (k-1) là (1/2), 

[ 
f(k)=\frac{1}{k}+\frac{1+(k-2)/2}{k} 
=\frac{1}{2}. 
] 

Do đó, câu trả lời đầu tiên là 1 khi (n=1) và (1/2) với mọi (n>1). Việc giảm tiêu chuẩn này cũng phù hợp với các phân tích đã công bố về vấn đề này. 

Câu hỏi thứ hai có thêm một lớp ngẫu nhiên, đó là vị trí của hành khách 1 trong thứ tự lên máy bay. Hãy để hành khách cuối cùng là hành khách chiếm vị trí cuối cùng trong hoán vị ngẫu nhiên. 

Có một cách đặc biệt rõ ràng để điều kiện cho hành khách 1. Hành khách 1 đứng cuối cùng với xác suất (1/m). Nếu điều đó xảy ra thì tất cả hành khách khác đã ngồi vào ghế riêng của mình nên hành khách 1 chọn thống nhất từ ​​một ghế còn lại, đó phải là ghế 1. Như vậy, hành khách cuối cùng đúng với xác suất 1 trong trường hợp này. 

Với xác suất (1-1/m), hành khách 1 không phải là người cuối cùng. Hãy xem xét thời điểm hành khách 1 bảng. Mọi hành khách lên máy bay trước anh ta nhất thiết phải ngồi vào ghế riêng của mình, bởi vì hành khách 1 là nguyên nhân duy nhất dẫn đến việc phân bổ chỗ ngồi không chính xác và anh ta vẫn chưa lên máy bay. Từ hành khách 1 trở đi, quá trình còn lại có cấu trúc tương tự như bài toán đầu tiên, ngoại trừ hành khách cuối cùng bây giờ là thành viên cuối cùng của hậu tố được sắp xếp ngẫu nhiên.

Xác suất để hậu tố này kết thúc đúng là (1/2). Như vậy 

\frac{1}{m}\cdot 1 
+ 
\left(1-\frac{1}{m}\right)\cdot\frac12. 
] 

Đơn giản hóa, 

[ 
g(m)=\frac{1}{m}+\frac{m-1}{2m} 
=\frac{m+1}{2m}. 
] 

Với (m=3), điều này cho ra (4/6=2/3), khớp với mẫu. Hình thức đóng tương tự được đưa ra bởi các phân tích biên tập độc lập về vấn đề. 

Sự đơn giản hóa quan trọng là chúng ta không bao giờ cần mô phỏng chuỗi hành khách phải di dời. Chuỗi chỉ quan trọng thông qua xác suất bất biến rằng hành khách cuối cùng cuối cùng của nó là chính xác, chính xác là (1/2) khi còn lại ít nhất hai hành khách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(m!,m^m)) | (O(m)) | Quá chậm | 
| Tối ưu | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và (m) cho test case hiện tại. Hai giá trị này thuộc về các bài toán con khác nhau nên chúng phải được xử lý độc lập. 
2. Tính câu trả lời đầu tiên là 1 khi (n=1) và 0,5 nếu ngược lại. Với mọi (n>1), quá trình dịch chuyển đệ quy sẽ để lại cho hành khách cuối cùng chính xác một trong hai khả năng đối xứng cho ghế đặc biệt, cho xác suất (1/2). 
3. Tính đáp án thứ hai với 

[ 
\frac{m+1}{2m}. 
] 

Công thức xuất phát từ việc điều kiện xem hành khách 1 có phải là hành khách cuối cùng hay không. Nếu anh ta là người cuối cùng, xảy ra với xác suất (1/m), thì hành khách cuối cùng chắc chắn đúng. Ngược lại, quá trình dịch chuyển còn lại đóng góp xác suất (1/2). 

1. In cả hai xác suất có đúng sáu chữ số sau dấu thập phân. Độ chính xác của dấu phẩy động của Python là quá đủ cho (m\le50) và câu lệnh đảm bảo rằng chữ số thập phân thứ bảy không chính xác ở ranh giới làm tròn có vấn đề. 

### Tại sao nó hoạt động 

Vấn đề đầu tiên có một chuỗi sự không chắc chắn. Hành khách 1 hoặc ngồi vào chỗ của mình và chấm dứt chuỗi ngay lập tức, hoặc ngồi vào chỗ của hành khách khác và chuyển vấn đề tương tự cho hành khách đó. Sự tái diễn cho thấy mọi bài toán có ít nhất hai hành khách đều có xác suất (1/2) kết thúc với hành khách cuối cùng đúng. 

Đối với vấn đề thứ hai, việc điều chỉnh vị trí của hành khách 1 sẽ tách biệt trường hợp ngoại lệ duy nhất. Nếu hành khách 1 đứng cuối cùng thì những người còn lại đã ngồi đúng và ghế cuối cùng còn lại là ghế 1 nên câu trả lời là 1. Nếu hành khách 1 không phải là người cuối cùng thì tất cả hành khách trước đó đều đúng và hậu tố chưa được giải quyết có cùng (1/2) xác suất ngồi vào ghế cuối cùng. Hai trường hợp loại trừ lẫn nhau này bao gồm mọi thứ tự lên máy bay có thể xảy ra, do đó tổng trọng số của chúng là xác suất cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for case_id in range(1, t + 1):
        n, m = map(int, input().split())

        first = 1.0 if n == 1 else 0.5
        second = (m + 1.0) / (2.0 * m)

        print(f"Case #{case_id}: {first:.6f} {second:.6f}")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào tuân theo định dạng nhiều trường hợp kiểm thử được yêu cầu. Mỗi trường hợp thử nghiệm là độc lập, do đó không có trạng thái nào cần được chuyển từ trường hợp này sang trường hợp tiếp theo. 

Biểu thức đầu tiên sử dụng kiểm tra rõ ràng cho (n=1). Giá trị 0,5 chỉ có giá trị khi có ít nhất hai hành khách. Ranh giới này là chi tiết triển khai chính cho câu trả lời đầu tiên. 

Biểu thức thứ hai thực hiện trực tiếp ((m+1)/(2m)). Việc sử dụng phép chia dấu phẩy động sẽ tránh được việc chia số nguyên ngẫu nhiên, mặc dù Python 3`/`toán tử đã tạo ra kết quả dấu phẩy động. 

Chuỗi được định dạng cuối cùng sử dụng sáu chữ số sau dấu thập phân, chính xác theo yêu cầu. Không có rủi ro tràn số nguyên vì số học duy nhất liên quan đến (m) có giá trị tối đa là 50. 

## Ví dụ đã hoạt động 

Mẫu chứa một trường hợp thử nghiệm,`n=2, m=3`. 

Đối với vấn đề đầu tiên, có hai hành khách. Hành khách 1 có hai lựa chọn có khả năng như nhau. Chọn ghế 1 thì hành khách 2 ngồi ghế 2, còn chọn ghế 2 thì buộc hành khách 2 ngồi vào ghế 1. 

| (n) | Câu trả lời đầu tiên | 
| --- | --- | 
| 1 | 1.000000 | 
| 2 | 0,500000 | 

Đối với bài toán thứ hai, (m=3). Hành khách 1 đứng cuối cùng với xác suất (1/3), trong trường hợp đó anh ta chắc chắn đúng. Ngược lại, với xác suất (2/3), hành khách 1 không phải là người cuối cùng và độ không chắc chắn còn lại có xác suất (1/2) khiến hành khách cuối cùng đúng. 

| (m) | (P(\text{hành khách 1 ở cuối})) | Xác suất đúng trong trường hợp đó | Xác suất đúng nếu không | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | 
| 3 | (1/3) | 1 | (1/2) | (1/3+2/3\cdot1/2=2/3) | 

Kết quả đầu ra là`Case #1: 0.500000 0.666667`, chính xác như trong mẫu. 

Dấu vết hữu ích thứ hai là`n=1, m=1`. Chỉ có một hành khách và một chỗ ngồi trong cả hai bài toán con. 

| (n) | Câu trả lời đầu tiên | (m) | Câu trả lời thứ hai | 
| --- | --- | --- | --- | 
| 1 | 1.000000 | 1 | 1.000000 | 

Ở đây cách giải thích lặp lại không có hành khách nào phải di chuyển cả. Hành khách duy nhất phải ngồi vào ghế duy nhất còn trống, vì vậy cả hai xác suất đều chính xác bằng 1. Công thức trực tiếp giải quyết vấn đề này mà không có trường hợp đặc biệt nào cho câu trả lời thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T)) | Mỗi trường hợp thử nghiệm chỉ yêu cầu một số phép tính số học không đổi. | 
| Không gian | (O(1)) | Không cần có mảng hoặc trạng thái tùy thuộc vào (n) hoặc (m). | 

Với (T\le50), chương trình chỉ thực hiện tối đa vài nghìn thao tác nguyên thủy. Giải pháp này thấp hơn nhiều so với giới hạn thời gian và bộ nhớ sẵn có, bất kể giới hạn thời gian ban đầu có được coi là rộng rãi đối với các ràng buộc hay không. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    t = int(input())

    out = []
    for case_id in range(1, t + 1):
        n, m = map(int, input().split())

        first = 1.0 if n == 1 else 0.5
        second = (m + 1.0) / (2.0 * m)

        out.append(f"Case #{case_id}: {first:.6f} {second:.6f}")

    return "\n".join(out)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    result = solve()

    sys.stdin = old_stdin
    input = sys.stdin.readline

    return result

assert run("1\n2 3\n") == "Case #1: 0.500000 0.666667", "sample"

assert run("1\n1 1\n") == "Case #1: 1.000000 1.000000", "minimum size"

assert run("1\n2 2\n") == "Case #1: 0.500000 0.750000", "two passengers"

assert run("1\n1 50\n") == "Case #1: 1.000000 0.510000", "n=1 boundary"

assert run("1\n50 50\n") == "Case #1: 0.500000 0.510000", "maximum and equal values"

assert run("2\n3 1\n50 1\n") == (
    "Case #1: 0.500000 1.000000\n"
    "Case #2: 0.500000 1.000000"
), "m=1 boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3`|`0.500000 0.666667`| Cung cấp mẫu và công thức chính | 
|`1 1`|`1.000000 1.000000`| Ranh giới kích thước tối thiểu | 
|`2 2`|`0.500000 0.750000`| Quy trình thứ nhất và thứ hai không cần thiết nhỏ nhất | 
|`1 50`|`1.000000 0.510000`| Ranh giới của câu trả lời đầu tiên (n=1) | 
|`50 50`|`0.500000 0.510000`| Giá trị lớn nhất và bằng (n,m) | 
|`3 1`,`50 1`|`0.500000 1.000000`| Ranh giới của câu trả lời thứ hai (m=1) | 

## Vỏ cạnh 

cho`1 1`, công thức đầu tiên phát hiện (n=1) và trả về 1. Công thức thứ hai cho ra ((1+1)/(2\cdot1)=1). Đầu ra cuối cùng là`Case #1: 1.000000 1.000000`. Không có hành khách bị di dời nào tồn tại nên không có chuỗi xác suất để giải quyết. 

Vì`2 2`, bài toán thứ nhất chỉ có hai lựa chọn ban đầu. Hành khách 1 chọn ghế 1 với xác suất (1/2), tạo ra hành khách cuối cùng đúng và chọn ghế 2 với xác suất (1/2), tạo ra hành khách cuối cùng không chính xác. Do đó giá trị đầu tiên là 0,5. Đối với vấn đề thứ hai, hành khách 1 đứng cuối cùng với xác suất (1/2), cho xác suất thành công là 1 trong trường hợp đó. Khi hành khách 1 đến đầu tiên, quy trình hai hành khách thông thường sẽ cho xác suất (1/2). Do đó kết quả là (1/2+1/2\cdot1/2=3/4), cho`Case #1: 0.500000 0.750000`. 

Vì`1 50`, câu trả lời đầu tiên phải giữ nguyên là 1 mặc dù giá trị (m) lớn, vì hai câu hỏi này độc lập. Câu trả lời thứ hai là ((50+1)/(100)=0,51). Sản lượng dự kiến ​​​​là`Case #1: 1.000000 0.510000`. Điều này phát hiện việc triển khai vô tình sử dụng cùng một tham số cho cả hai câu trả lời. 

Vì`50 50`, câu trả lời đầu tiên là 0,5 vì (n>1). Câu trả lời thứ hai là (51/100=0,51). Sản lượng dự kiến ​​​​là`Case #1: 0.500000 0.510000`. Đây là một phép kiểm tra ranh giới hữu ích vì cả hai tham số đều ở mức tối đa và bằng nhau, do đó việc hoán đổi (n) và (m) sẽ không hiển thị ở đây, trong khi công thức thứ hai không chính xác như (1/2) sẽ xảy ra. 

Vì`3 1`, hành khách 1 nhất thiết phải là hành khách duy nhất trong trường hợp khứ hồi, vì vậy câu trả lời thứ hai chính xác là 1. Câu trả lời đầu tiên là 0,5 vì ba hành khách tham gia vào quá trình đặt hàng cố định. Đầu ra là`Case #1: 0.500000 1.000000`. Điều này mắc phải sai lầm phổ biến là cho rằng câu trả lời thứ hai luôn gần bằng một nửa và quên mất sự đóng góp đặc biệt của hành khách số 1 là người cuối cùng.
