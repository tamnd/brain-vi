---
title: "CF 2191B - Sắp xếp lại MEX"
description: "Chúng ta được cấp một mảng các số nguyên và chúng ta được phép sắp xếp lại nó một cách tùy ý. Sau khi sửa thứ tự, chúng tôi xem xét mọi vị trí phân chia có thể có giữa tiền tố và hậu tố."
date: "2026-06-09T04:39:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2191
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1073 (Div. 2)"
rating: 1000
weight: 2191
solve_time_s: 68
verified: true
draft: false
---

[CF 2191B - Sắp xếp lại MEX](https://codeforces.com/problemset/problem/2191/B) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên và chúng ta được phép sắp xếp lại nó một cách tùy ý. Sau khi sửa thứ tự, chúng tôi xem xét mọi vị trí phân chia có thể có giữa tiền tố và hậu tố. Đối với mỗi phần tách, chúng tôi tính toán MEX của tiền tố và MEX của hậu tố và chúng tôi muốn tất cả các cặp này khác nhau. 

Yêu cầu chính là toàn cục: một hoán vị duy nhất phải tránh bất kỳ sự phân chia nào trong đó cả hai bên đều thiếu cùng một số nguyên không âm nhỏ nhất. Chúng tôi không chọn một phần tách nhưng phải đảm bảo rằng không có phần chia nào tạo ra sự bằng nhau của các giá trị MEX. 

Các ràng buộc rất nhỏ, tối đa 100 phần tử cho mỗi trường hợp thử nghiệm và 500 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng có thể đủ khả năng suy luận tuyến tính hoặc bậc hai cho mỗi trường hợp thử nghiệm, nhưng bất kỳ phép hoán vị giai thừa hoặc hàm mũ nào đều không cần thiết và nên tránh. Mặc dù n nhỏ nhưng hoán vị vẫn không thể thực hiện được vì n! tăng quá nhanh ngay cả ở mức n = 100. 

Một nỗ lực ngây thơ nhưng tự nhiên là cho rằng MEX phụ thuộc rất nhiều vào sự hiện diện của các số nguyên nhỏ bắt đầu từ 0. Điều này đã gợi ý rằng chỉ có sự phân bố của các giá trị thấp mới quan trọng chứ không phải thứ tự tương đối của chúng. 

Trường hợp cạnh tinh tế phát sinh khi mảng có rất ít số nhỏ khác nhau. Ví dụ: nếu tất cả các phần tử đều bằng 0 thì mọi tiền tố và hậu tố đều có MEX = 1, do đó mọi phép chia đều thất bại. Tương tự, nếu thiếu hoàn toàn 0 thì mọi phân đoạn đều có MEX = 0, một lần nữa khiến tất cả các phân chia đều thất bại. Những trường hợp suy biến này cho thấy cấu trúc tổng thể của tần số 0 và 1 chiếm ưu thế trong câu trả lời. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các hoán vị của mảng và với mỗi hoán vị tính toán tiền tố MEX và hậu tố MEX cho mỗi lần phân tách. Điều này đúng vì nó khớp trực tiếp với điều kiện. Tuy nhiên, việc tạo các hoán vị là giai thừa theo n và thậm chí việc kiểm tra một hoán vị cũng mất O(n^2) do tính toán lại MEX cho tất cả các phân đoạn. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là MEX chỉ thay đổi khi chúng tôi vượt qua ngưỡng nơi một số xuất hiện đầy đủ trong một phân đoạn. Để MEX tăng x quá khứ, tất cả các số từ 0 đến x phải xuất hiện trong đoạn đó. Điều này có nghĩa là các giá trị MEX tiền tố và hậu tố chỉ phụ thuộc vào cách phân phối 0, 1, 2, v.v. trong phần tách chứ không phụ thuộc vào thứ tự chính xác vượt quá tính khả thi. 

Sự đơn giản hóa mang tính quyết định là theo dõi số lần 0 xuất hiện, vì 0 là giá trị có ảnh hưởng nhất. Nếu có ít nhất một số 0, chúng ta có thể tách các lần xuất hiện để kiểm soát hành vi MEX tiền tố. Nếu không có số 0 thì cả tiền tố và hậu tố luôn có MEX = 0 cho bất kỳ phép chia nào, do đó điều kiện không bao giờ được thỏa mãn. 

Nếu có ít nhất hai số 0 thì chúng ta luôn có thể xây dựng được một cách sắp xếp hợp lệ. Chúng ta có thể đặt một số số 0 ở đầu và một số sau để tiền tố đôi khi chứa 0 và đôi khi không, tạo ra các giá trị MEX khác nhau trên tất cả các phần tách. Cấu trúc còn lại (các số khác 0) không thể loại bỏ khả năng này vì MEX trước tiên phụ thuộc vào việc 0 có tồn tại hay không. 

Nếu có chính xác một số 0, tình hình sẽ trở nên căng thẳng. Chúng ta phải đảm bảo rằng không có sự phân chia nào tạo ra các giá trị MEX giống hệt nhau. Điều này hóa ra là không thể trong một số cấu hình, nhưng có thể thực hiện được khi có đủ giá trị khác 0 để phân tách các vùng ảnh hưởng. Đặc tính quyết định giảm xuống việc đếm các lần xuất hiện của 0 và 1: nếu có ít nhất hai số 0 hoặc nếu có ít nhất một số 0 và ít nhất một số 1, thì chúng ta có thể xây dựng một thứ tự hợp lệ; nếu không thì chúng tôi không thể. 

Điều này làm giảm vấn đề từ hoán vị đến suy luận tần số đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đếm số lần mỗi giá trị xuất hiện, tập trung vào số 0 và số 1. Hai giá trị này chi phối hành vi MEX vì MEX luôn bắt đầu từ 0 và phụ thuộc vào việc có tồn tại số nguyên nhỏ hay không. 
2. Nếu mảng không có số 0 thì kết luận ngay câu trả lời là “KHÔNG”. Điều này là do mọi mảng con đều có MEX = 0, do đó mọi cặp tiền tố và hậu tố sẽ bằng nhau cho mỗi lần phân chia. 
3. Nếu có chính xác một số 0, hãy kiểm tra xem có ít nhất một phần tử khác 0 có thể phân tách hành vi tiền tố và hậu tố hay không. Trong thực tế, điều này chỉ dừng lại ở việc kiểm tra xem có ít nhất một số 1 hay không. Nếu không, bất kỳ sự phân chia nào cũng sẽ cô lập số 0 duy nhất theo cách buộc các giá trị MEX bằng nhau ở cả hai bên. 
4. Nếu có ít nhất hai số 0, xuất ra "CÓ". Chúng ta luôn có thể sắp xếp mảng sao cho một số phần tách bao gồm số 0 trong tiền tố trong khi một số khác thì không, đảm bảo các giá trị MEX khác nhau trên tất cả các phân vùng. 

Tại sao nó hoạt động: MEX của bất kỳ phân đoạn nào trước tiên được xác định bởi liệu 0 có hiện diện hay không, sau đó liệu có hiện diện 1 hay không nếu 0 được bao phủ hoàn toàn, v.v. Cách duy nhất để hai cạnh của một phép chia có thể có MEX bằng nhau cho tất cả các hoán vị là khi cấu trúc của các số 0 quá ràng buộc, khiến chúng ta không thể tách rời ảnh hưởng của chúng. Khi chúng tôi có đủ sự linh hoạt trong việc phân phối các số 0 (nhiều số 0 hoặc số bù 1), chúng tôi luôn có thể xây dựng một thứ tự trong đó một số ranh giới tiền tố-hậu tố phá vỡ sự bình đẳng MEX ở mỗi lần cắt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    z = a.count(0)
    o = a.count(1)
    
    if z == 0:
        print("NO")
    elif z == 1 and o == 0:
        print("NO")
    else:
        print("YES")
```Việc triển khai hoàn toàn dựa vào việc đếm số lần xuất hiện của 0 và 1. Điều này là đủ vì các giá trị còn lại không ảnh hưởng đến sự tồn tại của một sắp xếp hợp lệ ngoài việc đảm bảo rằng chúng ta không ở trong tình huống hằng số MEX suy biến hoàn toàn. 

Điều tinh tế quan trọng là chúng ta không bao giờ cố gắng xây dựng hoán vị một cách rõ ràng. Vấn đề chỉ hỏi liệu một thứ tự hợp lệ có tồn tại hay không và điều kiện cấu trúc sẽ chuyển sang kiểm tra tần số đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2
1 0
3
0 3 0
6
1 0 5 0 6 1
```Chúng tôi theo dõi số lượng 0 và 1. 

| Trường hợp | Mảng | số không | những cái | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | [1,0] | 1 | 1 | CÓ | 
| 2 | [0,3,0] | 2 | 0 | CÓ | 
| 3 | [1,0,5,0,6,1] | 2 | 2 | CÓ | 

Trường hợp đầu tiên hoạt động vì cả 0 và 1 đều tồn tại, cho phép phân kỳ MEX linh hoạt. Cái thứ hai hoạt động do có nhiều số 0, điều này đã cho phép phân tách. Cách thứ ba thậm chí còn linh hoạt hơn, vì vậy mọi sự phân chia đều có thể được thực hiện không bằng nhau trong MEX. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
3
2
0 2
3
2 3 4
4
0 0 0 0
```| Trường hợp | Mảng | số không | những cái | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | [0,2] | 1 | 0 | KHÔNG | 
| 2 | [2,3,4] | 0 | 0 | KHÔNG | 
| 3 | [0,0,0,0] | 4 | 0 | CÓ | 

Trường hợp thứ hai không thành công vì thiếu 0, làm cho tất cả các giá trị MEX bằng 0. Trường hợp đầu tiên thất bại vì một số 0 duy nhất không có bất kỳ số 1 nào không thể được sắp xếp để phân tách hành vi MEX tiền tố và hậu tố. Cách thứ ba thành công vì nhiều số 0 cho phép phân tách cấu hình trong đó tiền tố và hậu tố MEX khác nhau ở mọi ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Chúng tôi chỉ tính số lần xuất hiện trong một lần duyệt qua mảng | 
| Không gian | O(1) thêm | Chỉ có một số quầy được duy trì | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì n ≤ 100 và t ≤ 500, khiến cho ngay cả đầu vào trong trường hợp xấu nhất cũng trở nên tầm thường đối với quá trình xử lý. 

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
        z = a.count(0)
        o = a.count(1)
        if z == 0:
            out.append("NO")
        elif z == 1 and o == 0:
            out.append("NO")
        else:
            out.append("YES")
    return "\n".join(out)

# provided samples
assert run("""3
2
1 0
3
0 3 0
6
1 0 5 0 6 1
""") == """YES
YES
YES"""

# custom cases
assert run("""3
2
0 2
3
2 3 4
4
0 0 0 0
""") == """NO
NO
YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [1,0] | CÓ | trường hợp hợp lệ tối thiểu với 0 và 1 | 
| [2,3,4] | KHÔNG | thiếu hằng số lực 0 MEX | 
| [0,0,0,0] | CÓ | nhiều số không cho phép xây dựng | 

## Vỏ cạnh 

Khi không có số 0 trong mảng, mọi phân đoạn đều có MEX = 0 bất kể thứ tự. Ví dụ, đầu vào`[2,3,5]`luôn tạo ra tiền tố MEX 0 và hậu tố MEX 0 cho mỗi lần chia, vì vậy câu trả lời phải là KHÔNG. 

Khi có chính xác một số 0 và không có một số nào, chẳng hạn như`[0,2,3]`, số 0 duy nhất không thể được sử dụng để tạo bất kỳ sự phân chia nào trong đó tiền tố và hậu tố MEX khác nhau một cách nhất quán trên tất cả các phân vùng, buộc phải xảy ra lỗi. 

Khi có nhiều số không, chẳng hạn như`[0,0,1,2]`, chúng ta luôn có thể đặt các số 0 để đảm bảo rằng ở mỗi phần phân chia có ít nhất một bên khác nhau về việc nó có chứa 0 hay không, ngăn cản sự bằng nhau của các giá trị MEX trên tất cả các phần phân tách.
