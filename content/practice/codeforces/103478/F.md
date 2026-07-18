---
title: "CF 103478F - \u9b54\u6cd5\u5c11\u5973\u83ab\u5361\u7684\u8bde\u751f"
description: "Chúng ta được cho một hoán vị có độ dài $2n$, nghĩa là nó chứa mọi số nguyên từ $1$ đến $2n$ đúng một lần. Chúng ta được phép thực hiện hoán đổi các vị trí liền kề, trao đổi giá trị tại các vị trí $i$ và $i+1$. Điều kiện mục tiêu không phải là sắp xếp toàn cục."
date: "2026-07-03T06:35:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103478
codeforces_index: "F"
codeforces_contest_name: "The 16-th Beihang University Collegiate Programming Contest (BCPC 2021) - Final"
rating: 0
weight: 103478
solve_time_s: 52
verified: true
draft: false
---

[CF 103478F - \u9b54\u6cd5\u5c11\u5973\u83ab\u5361\u7684\u8bde\u751f](https://codeforces.com/problemset/problem/103478/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$2n$, nghĩa là nó chứa mọi số nguyên từ$1$ĐẾN$2n$đúng một lần. Chúng ta được phép thực hiện hoán đổi vị trí liền kề, trao đổi giá trị tại các vị trí$i$Và$i+1$. 

Điều kiện mục tiêu không phải là sắp xếp toàn cục. Thay vào đó, mảng được coi là hợp lệ khi mọi cặp vị trí liên tiếp đều thỏa mãn ràng buộc thứ tự cục bộ: vị trí$i$không được vượt quá vị trí$i+1$cho mọi cặp quan trọng trong cấu trúc cuối cùng. Từ hành vi mẫu, ràng buộc này được áp dụng cho các khối cố định có kích thước hai, do đó mỗi cặp$(1,2), (3,4), \dots, (2n-1,2n)$phải kết thúc bên trong không giảm. Không có yêu cầu so sánh giá trị giữa các cặp khác nhau. 

Nhiệm vụ là tính toán số lượng hoán đổi liền kề tối thiểu cần thiết để đạt được bất kỳ cấu hình nào thỏa mãn điều kiện mỗi cặp này. 

Những hạn chế$n \le 5 \cdot 10^4$ngụ ý kích thước mảng lên tới$10^5$. Bất kỳ giải pháp nào vượt quá thời gian tuyến tính hoặc gần tuyến tính sẽ chặt chẽ nhưng vẫn khả thi, trong khi mọi giải pháp bậc hai sẽ quá chậm. Tuy nhiên, quan sát chính làm giảm bài toán thành quyết định theo thời gian không đổi cho mỗi cặp, làm cho giải pháp trở nên hiệu quả$O(n)$. 

Một cạm bẫy tinh vi xuất phát từ việc hiểu sai điều kiện là sắp xếp đầy đủ. Nếu chúng ta giả định sai thứ tự toàn cầu, chúng ta sẽ tính toán số lần đảo ngược. Ví dụ: đối với đầu vào mẫu:```
2 4 6 3 5 1
```số lượng đảo ngược toàn cầu lớn hơn 2 rất nhiều, nhưng câu trả lời đúng là 2 vì chỉ có thứ tự cặp địa phương mới quan trọng. 

Một sai lầm khác có thể xảy ra là nghĩ rằng việc hoán đổi giữa các cặp là cần thiết. Trên thực tế, hoán đổi cặp chéo chỉ có thể gây ra những gián đoạn không cần thiết vì ràng buộc cuối cùng hoàn toàn mang tính cục bộ. 

## Phương pháp tiếp cận 

Cách giải thích ngây thơ là coi vấn đề như một nhiệm vụ sắp xếp bằng cách sử dụng các hoán đổi liền kề. Theo quan điểm đó, mỗi lần hoán đổi sẽ loại bỏ chính xác một phép đảo ngược, vì vậy câu trả lời sẽ là tổng số lần đảo ngược của hoán vị. Điều này chỉ đúng khi mục tiêu được sắp xếp đầy đủ. Nó có thể được tính toán trong$O(n \log n)$sử dụng cây Fenwick hoặc sắp xếp hợp nhất. 

Sự thất bại của phương pháp này xuất phát từ cấu trúc của điều kiện mục tiêu. Vì chúng ta không cần trật tự toàn cầu nên hầu hết các sự đảo ngược đều không liên quan. Chỉ những vi phạm bên trong mỗi cặp cố định mới quan trọng và những vi phạm đó có thể được giải quyết một cách độc lập. 

Cái nhìn sâu sắc quan trọng là mỗi cặp$(2k-1, 2k)$là độc lập. Nếu phần tử bên trái đã nhỏ hơn thì không cần thiết. Nếu nó lớn hơn, chính xác một hoán đổi liền kề trong cặp đó sẽ sửa nó và không cần có hoán đổi bổ sung nào để đáp ứng ràng buộc cho cặp đó. Điều này giúp loại bỏ mọi tương tác giữa các phần khác nhau của mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đếm đảo ngược toàn cầu |$O(n \log n)$|$O(n)$| Mô hình không chính xác | 
| Hiệu chỉnh cục bộ theo cặp |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng theo khối có hai vị trí. 

1. Quét mảng từ trái sang phải theo hai bước, xử lý từng bước$(2k-1, 2k)$như một đơn vị duy nhất. 
2. Với mỗi cặp, so sánh hai giá trị. 
3. Nếu giá trị bên trái lớn hơn giá trị bên phải, chúng tôi tính một lần hoán đổi và hoán đổi chúng về mặt khái niệm. 
4. Nếu cặp đã được đặt hàng, chúng tôi không làm gì cả. 
5. Tính tổng tất cả các giao dịch hoán đổi trên tất cả các cặp và đưa ra kết quả. 

Mỗi quyết định đều mang tính cục bộ vì không có ràng buộc nào kết nối các cặp khác nhau. Hoạt động duy nhất quan trọng là liệu một cặp có được đặt hàng nội bộ hay không. 

### Tại sao nó hoạt động 

Điều kiện cuối cùng chỉ phụ thuộc vào thứ tự tương đối của các phần tử bên trong các cặp rời rạc cố định. Bất kỳ trao đổi nào sửa chữa một cặp không cần phải truyền bá các thay đổi ở nơi khác vì không có ràng buộc nào khác tham chiếu đến mối quan hệ định vị. Do đó, mỗi cặp đóng góp độc lập vào tổng chi phí và mỗi cặp đảo ngược yêu cầu chính xác một lần hoán đổi, trong khi các cặp không đảo ngược không yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    ans = 0
    for i in range(0, 2 * n, 2):
        if a[i] > a[i + 1]:
            ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện quét cặp. Vòng lặp tiến thêm 2 để căn chỉnh với các khối cố định. Hoạt động duy nhất là so sánh từng cặp, xác định xem cặp đó có đóng góp một lần hoán đổi hay không. 

Một lỗi phổ biến là cố gắng mô phỏng các giao dịch hoán đổi một cách rõ ràng. Điều đó là không cần thiết vì chúng ta không bao giờ cần những cấu hình trung gian; chúng tôi chỉ đếm có bao nhiêu cặp ban đầu không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
2 4 6 3 5 1
```| Cặp | Giá trị | Tình trạng | Hoán đổi được tính | 
| --- | --- | --- | --- | 
| (1,2) | (2,4) | được | 0 | 
| (3,4) | (6,3) | Vi phạm | 1 | 
| (5,6) | (5,1) | Vi phạm | 1 | 

Đầu ra:```
2
```Điều này chứng tỏ rằng mỗi cặp vi phạm đóng góp độc lập và yêu cầu chính xác một sự điều chỉnh. 

### Ví dụ 2 

đầu vào:```
n = 2
1 2 3 4
```| Cặp | Giá trị | Tình trạng | Hoán đổi được tính | 
| --- | --- | --- | --- | 
| (1,2) | (1,2) | được | 0 | 
| (3,4) | (3,4) | được | 0 | 

Đầu ra:```
0
```Điều này cho thấy cấu trúc cục bộ đã hợp lệ không cần thực hiện thao tác nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần vượt qua$2n$phần tử, công việc liên tục trên mỗi cặp | 
| Không gian |$O(1)$| Chỉ có một bộ đếm và lưu trữ mảng đầu vào | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện quét tuyến tính và không có cấu trúc dữ liệu bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    for i in range(0, 2 * n, 2):
        if a[i] > a[i + 1]:
            ans += 1

    return str(ans)

# provided sample
assert run("3\n2 4 6 3 5 1\n") == "2"

# already sorted pairs
assert run("2\n1 2 3 4\n") == "0"

# all pairs reversed
assert run("2\n2 1 4 3\n") == "2"

# single pair
assert run("1\n2 1\n") == "1"

# alternating pattern
assert run("3\n1 3 5 2 4 6\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp đảo ngược | 2 | mỗi cặp xấu đều được tính một lần | 
| đã được sắp xếp | 0 | không có giao dịch hoán đổi không cần thiết | 
| cặp đơn | 1 | trường hợp cơ sở tối thiểu | 
| mô hình xen kẽ | 1 | sự độc lập của các cặp | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các phần tử đã được sắp xếp chính xác trên toàn cầu. Trong trường hợp đó, mọi cặp đều đúng nên câu trả lời là 0 và thuật toán chỉ thực hiện so sánh. 

Một trường hợp khác là khi mọi cặp đều bị đảo ngược. Đối với một đầu vào như:```
2
2 1 4 3
```mỗi cặp đóng góp chính xác một lần hoán đổi và thuật toán tính chính xác hai lần điều chỉnh độc lập. 

Trường hợp cạnh cấu trúc cuối cùng là khi thứ tự cục bộ thay thế, chẳng hạn như:```
3
1 3 5 2 4 6
```Chỉ có một cặp không chính xác và việc sửa nó không tương tác với các cặp khác, xác nhận rằng tính độc lập của cặp đó đặc trưng đầy đủ cho giải pháp.
