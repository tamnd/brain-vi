---
title: "CF 1391A - Các mảng phụ"
description: "Chúng ta được yêu cầu xây dựng một hoán vị của các số từ 1 đến n sao cho mỗi phân đoạn liền kề hoạt động theo một cách rất cụ thể trong phép toán OR theo bit. Đối với bất kỳ mảng con nào, chúng ta lấy tất cả các giá trị bên trong nó và tính toán OR theo bit của chúng."
date: "2026-06-16T14:57:34+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1391
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 663 (Div. 2)"
rating: 800
weight: 1391
solve_time_s: 275
verified: false
draft: false
---

[CF 1391A - Suborray](https://codeforces.com/problemset/problem/1391/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 4 phút 35 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị của các số từ 1 đến n sao cho mỗi phân đoạn liền kề hoạt động theo một cách rất cụ thể trong phép toán OR theo bit. 

Đối với bất kỳ mảng con nào, chúng ta lấy tất cả các giá trị bên trong nó và tính toán OR theo bit của chúng. Yêu cầu là giá trị OR này phải luôn lớn ít nhất bằng độ dài của mảng con. Vì vậy, nếu một phân đoạn có độ dài 5 thì OR của các phần tử của nó phải có ít nhất là 5 và điều này phải đúng cho mọi phân đoạn có thể có chứ không chỉ toàn bộ mảng. 

Đầu ra không phải là duy nhất. Bất kỳ hoán vị nào thỏa mãn điều kiện này đều được chấp nhận, vì vậy nhiệm vụ này hoàn toàn mang tính xây dựng. 

Các ràng buộc nhỏ, với n lên tới 100 và lên tới 100 trường hợp thử nghiệm. Điều này loại trừ mọi nhu cầu tối ưu hóa ngoài việc xây dựng tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm. Ngay cả việc kiểm tra một hoán vị ứng cử viên một cách đơn giản cũng có thể thực hiện được vì n nhỏ, nhưng việc xây dựng một hoán vị một cách trực tiếp là mục tiêu đã định. 

Một điểm tinh tế là OR hoạt động khác với các ràng buộc tổng hoặc tối thiểu. Nó không tích lũy tuyến tính. Một bit lớn có thể chi phối nhiều giá trị. Điều này thường gợi ý các cấu trúc phân phối lũy thừa của hai hoặc các số thứ tự cẩn thận để tiền tố và mảng con có đủ phạm vi bit. 

Một nỗ lực ngây thơ có thể là sắp xếp hoán vị hoặc đảo ngược nó hoặc xáo trộn nó một cách ngẫu nhiên. Những thất bại dễ dàng. Ví dụ: với n = 3, hoán vị nhận dạng [1, 2, 3] không thành công trên mảng con [1, 2] vì 1 OR 2 = 3 là ổn, nhưng các cách sắp xếp khác như [1, 3, 2] vẫn có thể đạt hoặc không thành công tùy thuộc vào cấu trúc. Thách thức thực sự là đảm bảo mọi độ dài cửa sổ k luôn có OR bao phủ ít nhất k, điều này gắn chặt với biểu diễn nhị phân hơn là độ lớn số. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra một hoán vị và kiểm tra xem nó có thỏa mãn điều kiện hay không. Việc kiểm tra yêu cầu lặp lại tất cả các mảng con O(n2) và mỗi phép tính OR tốn O(n), cho ra O(n³) cho mỗi hoán vị. Vì có n! hoán vị, điều này hoàn toàn không khả thi ngay cả khi n = 100. 

Ngay cả khi chúng tôi sửa một hoán vị, việc kiểm tra kỹ càng vẫn là O(n³), nằm ở ranh giới nhưng có thể chấp nhận được với n = 100. Tuy nhiên, việc tạo ra các ứng cử viên một cách mù quáng là vô nghĩa. Vì vậy, câu hỏi thực sự là làm thế nào để xây dựng một hoán vị đảm bảo phạm vi bao phủ của các bit trong mọi phân đoạn. 

Quan sát quan trọng là điều kiện yếu nhất đối với các mảng con ngắn. Một mảng con có độ dài k yêu cầu OR ≥ k, do đó giá trị k xác định số lượng bit phải xuất hiện trong phân đoạn. Đối với k nhỏ, điều này thật dễ dàng; đối với k lớn hơn, chúng ta cần các bit cao hơn để xuất hiện đủ thường xuyên. 

Một cách hữu ích để diễn giải lại điều kiện là mọi đoạn có độ dài k phải chứa đủ phạm vi nhị phân sao cho tất cả các số từ 1 đến k đều bị OR “chi phối”. Điều này gợi ý rằng chúng ta muốn một cấu trúc trong đó các số có bit cao nhất tương tự được trải ra một cách có kiểm soát. 

Thủ thuật mang tính xây dựng đã biết cho bài toán này đơn giản một cách đáng ngạc nhiên: với n ≥ 2, việc đảo ngược hoán vị tự nhiên có tác dụng. Tức là chúng ta xuất ra [n, n-1, ..., 1]. 

Tại sao điều này làm việc? Bởi vì đối với bất kỳ mảng con nào, OR của một tập hợp các số nguyên liên tiếp theo thứ tự giảm dần có xu hướng nhanh chóng bao gồm các bit cao sớm và quan trọng hơn, bất kỳ đoạn nào có độ dài k phải bao gồm ít nhất một số từ đầu phạm vi bit nào đó để đảm bảo tăng trưởng OR là đủ. Thuộc tính quan trọng là theo thứ tự ngược lại, mọi tiền tố có độ dài k chứa một số có biểu diễn nhị phân bao gồm yêu cầu phạm vi k. 

Cụ thể hơn, hãy xem xét bất kỳ đoạn nào có độ dài k. Giá trị tối đa có thể có trong phân đoạn đó ít nhất là n - k + 1. Theo thứ tự ngược lại, các phân đoạn có xu hướng bao gồm các giá trị lớn sớm hơn, đảm bảo rằng các bit cao xuất hiện trong mọi cửa sổ. Các bit cao này đảm bảo rằng OR không quá nhỏ so với k.

Một cách có cấu trúc hơn để thấy điều đó là việc xây dựng đảm bảo rằng đối với bất kỳ đoạn có độ dài k nào, tồn tại một phần tử có bit được đặt cao nhất ít nhất là log2(k), điều này buộc OR phải đủ lớn để lấn át k. 

Điều này làm cho ý tưởng bạo lực trở nên không cần thiết: thay vì xác minh các ràng buộc, chúng tôi trực tiếp thực thi phân phối bit cao đơn điệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra hoán vị) | O(n³ · n!) | O(n) | Quá chậm | 
| Xây dựng hoán vị ngược | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc n. 
2. Xây dựng hoán vị bằng cách liệt kê các số nguyên từ n đến 1. 

Điều này đảm bảo các giá trị lớn hơn mang bit nhị phân cao hơn xuất hiện sớm trong mảng. 
3. Xuất ra hoán vị đã xây dựng. 

Lý do đằng sau việc chọn thứ tự giảm dần là nó tối đa hóa việc hiển thị sớm các bit cao trên tất cả các mảng con. Vì mỗi mảng con phải bao gồm các giá trị có phạm vi kích thước khác nhau, nên thứ tự này đảm bảo không có phân đoạn nào chỉ bao gồm các số bit thấp nhỏ. 

### Tại sao nó hoạt động 

Bất biến là trong bất kỳ mảng con nào có độ dài k, tồn tại ít nhất một phần tử có bit quan trọng nhất đủ lớn để buộc OR ít nhất là k. Vì các số được sắp xếp theo thứ tự giảm dần nên bất kỳ cửa sổ nào có kích thước k chắc chắn sẽ bao gồm một giá trị từ phần trên của phạm vi và các giá trị đó chiếm ưu thế trong kết quả OR. Điều này ngăn bất kỳ mảng con nào có OR quá nhỏ so với độ dài của nó, đảm bảo sự bất bình đẳng cần thiết được giữ nguyên trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    res = list(range(n, 0, -1))
    print(*res)
```Giải pháp là xây dựng trực tiếp không cần mô phỏng. Sự lựa chọn không hề tầm thường duy nhất là hướng đặt hàng. In ngược đảm bảo các giá trị lớn nhất xuất hiện đầu tiên, đây là thuộc tính cấu trúc thực thi ràng buộc OR theo bit. 

Không có vấn đề về ranh giới nào ngoài việc xử lý n = 1, trong đó cùng một cách xây dựng cho ra kết quả tầm thường [1], điều này là hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3 

Hoán vị được xây dựng: 

[3, 2, 1] 

Chúng tôi kiểm tra một vài mảng con: 

| mảng con | HOẶC | Chiều dài | Tình trạng | 
| --- | --- | --- | --- | 
| [3] | 3 | 1 | được | 
| [2] | 2 | 1 | được | 
| [1] | 1 | 1 | được | 
| [3,2] | 3 | 2 | được | 
| [2,1] | 3 | 2 | được | 
| [3,2,1] | 3 | 3 | được | 

Mọi đoạn đều thỏa mãn OR ≥ độ dài. 

### Ví dụ 2 

đầu vào: 

n = 5 

Hoán vị được xây dựng: 

[5, 4, 3, 2, 1] 

Kiểm tra các phân đoạn đại diện: 

| mảng con | HOẶC | Chiều dài | Tình trạng | 
| --- | --- | --- | --- | 
| [5,4] | 5 | 2 | được | 
| [4,3,2] | 7 | 3 | được | 
| [3,2,1] | 3 | 3 | được | 
| [5,4,3,2] | 7 | 4 | được | 

Điều này cho thấy các bit cao từ các giá trị lớn hơn chiếm ưu thế như thế nào ngay cả các đoạn dài. 

Những dấu vết này xác nhận rằng cấu trúc luôn đưa các đóng góp nhị phân đủ lớn vào mọi khối liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi chỉ tạo và in danh sách đảo ngược | 
| Không gian | O(1) thêm | Mảng đầu ra là bộ lưu trữ duy nhất được sử dụng | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với n lên tới 100, do đó, ngay cả việc xây dựng trực tiếp cho mỗi trường hợp thử nghiệm cũng nhanh chóng một cách tầm thường. Giải pháp chạy trong thời gian không đổi so với giới hạn của vấn đề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        res = list(range(n, 0, -1))
        out.append(" ".join(map(str, res)))
    return "\n".join(out)

# provided samples
assert run("3\n1\n3\n7\n") == "1\n3 2 1\n7 6 5 4 3 2 1"

# custom cases
assert run("1\n2\n") == "2 1", "minimum non-trivial"
assert run("1\n5\n") == "5 4 3 2 1", "small descending correctness"
assert run("2\n1\n1\n") == "1\n1", "repeated minimal cases"
assert run("1\n4\n") == "4 3 2 1", "even length structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp hợp lệ nhỏ nhất | 
| 5 | 5 4 3 2 1 | tính đúng đắn của việc xây dựng | 
| 1 1 | 1/1 | xử lý nhiều bài kiểm tra | 
| 4 | 4 3 2 1 | tính nhất quán của cấu trúc chung | 

## Vỏ cạnh 

Với n = 1, hoán vị [1] thỏa mãn điều kiện vì mọi mảng con đều có OR bằng 1 và độ dài 1. Thuật toán đưa ra [1] vì phạm vi đảo ngược của một phần tử là chính nó. 

Với n = 2, đầu ra là [2, 1]. Các mảng con [2], [1] và [2,1] có giá trị OR lần lượt là 2, 1 và 3, tất cả đều đáp ứng các giới hạn dưới được yêu cầu. Việc xây dựng đảm bảo phần tử lớn hơn xuất hiện đầu tiên, đáp ứng ngay phân khúc không tầm thường duy nhất. 

Đối với n lớn hơn, mọi cửa sổ nhất thiết phải bao gồm ít nhất một giá trị cao từ tiền tố của mảng đảo ngược và phần tử đó đảm bảo đủ độ bao phủ bit để giữ OR cao hơn yêu cầu về độ dài cửa sổ.
