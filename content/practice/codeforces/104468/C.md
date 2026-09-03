---
title: "CF 104468C - Hoán vị hữu ích Ammar"
description: "Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến N sao cho có đúng một cặp liền kề có tổng lẻ. Mọi cặp liền kề khác phải có tổng chẵn. Tổng liền kề chỉ là số lẻ khi một số chẵn và số kia là số lẻ."
date: "2026-06-30T12:55:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104468
codeforces_index: "C"
codeforces_contest_name: "The 2023 Damascus University Collegiate Programming Contest"
rating: 0
weight: 104468
solve_time_s: 84
verified: false
draft: false
---

[CF 104468C - Hoán vị Ammar-utiful](https://codeforces.com/problemset/problem/104468/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến N sao cho có đúng một cặp liền kề có tổng lẻ. Mọi cặp liền kề khác phải có tổng chẵn. 

Tổng liền kề chỉ là số lẻ khi một số chẵn và số kia là số lẻ. Vì vậy, điều kiện thực sự là về việc kiểm soát số lần chuyển đổi chẵn lẻ giữa các lân cận trong hoán vị. Chúng ta cần chính xác một vị trí trong đó số chẵn nằm cạnh số lẻ và ở mọi vị trí khác các phần tử liền kề phải có cùng tính chẵn lẻ. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cho một giá trị N. Với mỗi N, chúng ta phải xuất ra bất kỳ hoán vị hợp lệ nào. 

Các ràng buộc cho phép N lên tới 100000 cho mỗi thử nghiệm, với tổng số tiền lên tới 100000. Điều này có nghĩa là chúng ta cần cấu trúc O(N) cho mỗi trường hợp thử nghiệm, vì bất cứ điều gì như O(N log N) lặp lại qua nhiều thử nghiệm vẫn ổn nhưng không cần thiết và hoán vị vũ lực là không thể. Việc xây dựng quay lui hoặc dựa trên tìm kiếm sẽ ngay lập tức thất bại vì không gian hoán vị là giai thừa. 

Một vấn đề tế nhị nảy sinh khi nghĩ về việc phân nhóm chẵn lẻ. Nếu chúng ta tách riêng số chẵn và số lẻ thì trong mỗi nhóm, tất cả các tổng liền kề đều tự động chẵn. Nơi nguy hiểm duy nhất là ranh giới giữa khối chẵn và khối lẻ. Điều này ngay lập tức gợi ý rằng cấu trúc hoán vị được kiểm soát hoàn toàn bằng cách chúng ta ghép các khối chẵn lẻ này. 

Các trường hợp cạnh xuất hiện với N nhỏ. Với N = 2, chúng ta phải kiểm tra tính khả thi một cách rõ ràng. Các hoán vị duy nhất là [1,2] và [2,1], cả hai đều tạo ra chính xác một cặp liền kề và cặp đó là số lẻ+chẵn = tổng lẻ, vì vậy điều kiện được giữ nguyên. Đối với N = 3, chúng ta phải đảm bảo vẫn có thể tách biệt chính xác một lân cận chẵn lẻ hỗn hợp, điều này cũng có thể thực hiện được nhưng cần sắp xếp chính xác. 

Một cách tiếp cận ngây thơ sẽ thử các hoán vị ngẫu nhiên hoặc hoán đổi tham lam, nhưng nếu không thực thi cấu trúc chẵn lẻ toàn cầu, nó sẽ dễ dàng tạo ra nhiều lân cận tổng lẻ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là tạo ra các hoán vị và kiểm tra điều kiện. Điều này hoạt động bằng cách liệt kê tất cả các hoán vị từ 1 đến N và xác minh tổng kề. Xác minh là O(N), nhưng có N! hoán vị, do đó ngay cả N = 10 cũng không thể thực hiện được. Không gian tìm kiếm bùng nổ ngay lập tức. 

Quan sát quan trọng là cấu trúc chẵn lẻ xác định đầy đủ điều kiện. Bên trong một khối liền kề gồm tất cả các số chẵn hoặc tất cả các số lẻ, mọi tổng liền kề đều là số chẵn. Cách duy nhất để có được số lẻ là đặt số lẻ bên cạnh số chẵn. Vì vậy chúng ta cần chính xác một ranh giới giữa khối lẻ và khối chẵn. 

Nếu chúng ta nhóm tất cả các tỷ lệ lại với nhau và tất cả các số chẵn lại với nhau, chúng ta thường nhận được chính xác một ranh giới nếu chúng ta ghép các khối một lần. Tuy nhiên, nếu chúng ta không cẩn thận với việc sắp xếp thứ tự bên trong các khối, chúng ta có thể vô tình đưa ra các chuyển đổi bổ sung chỉ khi chúng ta trộn các giá trị chẵn lẻ, nhưng vì chúng ta không bao giờ trộn lẫn trong các khối nên số lượng thay đổi tính chẵn lẻ chính xác là một. 

Vì vậy, việc xây dựng giảm xuống việc đặt hàng tỷ lệ cược và số chẵn riêng biệt và nối chúng theo một hướng. Mọi đơn đặt hàng nội bộ đều có tác dụng; đơn giản nhất là thứ tự tăng dần. 

Chúng ta cũng phải đảm bảo rằng không có khối nào trống theo cách vi phạm điều kiện. Nếu N = 1 không liên quan do các ràng buộc và với N ≥ 2 thì cả hai bộ chẵn lẻ đều không trống, do đó phép nối luôn hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N!) | O(N) | Quá chậm | 
| Xây dựng nhóm chẵn lẻ | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hoán vị bằng cách sử dụng tách chẵn lẻ.

1. Chia các số từ 1 đến N thành hai dãy chẵn và lẻ. Điều này tách biệt tính chẵn lẻ để chúng ta có thể kiểm soát nơi xảy ra các vùng lân cận lẻ-chẵn. 
2. Xuất ra tất cả các số lẻ theo thứ tự tăng dần. Bên trong đoạn này, mọi tổng liền kề đều là số chẵn vì lẻ + lẻ luôn là số chẵn. 
3. Xuất ra tất cả các số chẵn theo thứ tự tăng dần. Tương tự, trong các số chẵn, mọi tổng liền kề đều là số chẵn. 
4. Tỷ lệ cược nối tiếp theo số chẵn. 
5. Điều này tạo ra chính xác một ranh giới giữa số lẻ cuối cùng và số chẵn đầu tiên. Ranh giới đó đóng góp chính xác một tổng lẻ vì nó là số lẻ + số chẵn. 

Tại sao nó hoạt động 

Tất cả các cặp liền kề bên trong đoạn lẻ và bên trong đoạn chẵn đều có điểm cuối chẵn lẻ bằng nhau, do đó tổng của chúng luôn chẵn. Điểm liền kề duy nhất kết nối các điểm chẵn lẻ khác nhau là ranh giới duy nhất giữa hai phân đoạn. Vì việc xây dựng tạo ra chính xác một ranh giới như vậy nên điều kiện được thỏa mãn đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    
    odds = []
    evens = []
    
    for i in range(1, n + 1):
        if i % 2:
            odds.append(i)
        else:
            evens.append(i)
    
    res = odds + evens
    print(*res)
```Giải pháp này hoạt động bằng cách xây dựng rõ ràng hai danh sách dựa trên tính chẵn lẻ. Vòng lặp từ 1 đến N là tuyến tính và mỗi số được phân loại thành số lẻ hoặc số chẵn. Việc ghép nối được thực hiện một lần cho mỗi trường hợp thử nghiệm. 

Chi tiết triển khai chính là chúng tôi không bao giờ xen kẽ hai danh sách. Bất kỳ sự xen kẽ nào cũng có nguy cơ tạo ra nhiều vùng lân cận chẵn-lẻ, vi phạm yêu cầu. Giữ chúng tách biệt đảm bảo một điểm chuyển tiếp duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

N = 7 

Chúng tôi xây dựng: 

| Bước | Tỷ lệ cược | Sự kiện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | [1,3,5,7] | [] | [] | 
| 2 | [1,3,5,7] | [2,4,6] | 1 3 5 7 2 4 6 | 

Hoán vị cuối cùng: 1 3 5 7 2 4 6 

Chỉ kề giữa 7 và 2 là số lẻ + chẵn, tạo ra một tổng lẻ. 

### Ví dụ 2 

đầu vào: 

N = 6 

| Bước | Tỷ lệ cược | Sự kiện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | [1,3,5] | [2,4,6] | 1 3 5 2 4 6 | 

Hoán vị cuối cùng: 1 3 5 2 4 6 

Vùng lân cận hỗn hợp duy nhất là từ 5 đến 2. 

Điều này xác nhận rằng bất kể N, việc xây dựng tạo ra chính xác một chuyển tiếp chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) mỗi lần kiểm tra | Mỗi số được xử lý một lần và xuất ra một lần | 
| Không gian | O(N) | Lưu trữ danh sách tỷ lệ cược và chẵn | 

Tổng N trên tất cả các trường hợp thử nghiệm tối đa là 100000, do đó, việc xây dựng tuyến tính cho mỗi trường hợp thử nghiệm dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ cũng tuyến tính và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        odds = []
        evens = []
        for i in range(1, n + 1):
            if i % 2:
                odds.append(i)
            else:
                evens.append(i)
        res = odds + evens
        out.append(" ".join(map(str, res)))
    return "\n".join(out)

# provided sample
assert run("1\n7\n") == "1 3 5 7 2 4 6"

# minimum size
assert run("1\n2\n") in ["1 2", "2 1"]

# small case
assert run("1\n3\n") in ["1 3 2", "3 1 2", "1 3 2"]

# even N
assert run("1\n6\n") == "1 3 5 2 4 6"

# multiple tests
assert run("2\n4\n5\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=2 | bất kỳ hợp lệ | hành vi ranh giới tối thiểu | 
| N=3 | chia hợp lệ | cấu trúc không tầm thường nhỏ nhất | 
| N=6 | 1 3 5 2 4 6 | tính đúng đắn của phân vùng chẵn/lẻ | 
| nhiều bài kiểm tra | đầu ra nhất quán | xử lý T | 

## Vỏ cạnh 

Với N = 2, thuật toán tạo ra tỷ lệ cược = [1], chẵn = [2], do đó đầu ra là [1,2]. Giá trị kề duy nhất là 1 + 2, là số lẻ nên điều kiện đúng một lần. 

Với N = 3, tỷ lệ cược = [1,3], chẵn = [2], do đó đầu ra là [1,3,2]. Các tổng liền kề là 1+3 = chẵn và 3+2 = lẻ, cho chính xác một chỉ số hợp lệ. 

Đối với N = 1 không được phép bởi các ràng buộc, do đó không cần xử lý đặc biệt. 

Đối với tất cả N lớn hơn, cấu trúc vẫn giống hệt nhau: một ranh giới chẵn lẻ duy nhất đảm bảo chính xác một tổng lẻ liền kề và không có khối bên trong nào gây ra bất kỳ vi phạm nào.
