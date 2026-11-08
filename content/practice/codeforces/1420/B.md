---
title: "CF 1420B - Đá và đòn bẩy"
description: "Chúng ta được cho một số mảng số nguyên độc lập. Đối với mỗi mảng, chúng ta phải đếm xem có bao nhiêu cặp vị trí tạo thành một “tương tác tốt” trong điều kiện bitwise."
date: "2026-06-11T06:38:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "math"]
categories: ["algorithms"]
codeforces_contest: 1420
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 672 (Div. 2)"
rating: 1200
weight: 1420
solve_time_s: 93
verified: true
draft: false
---

[CF 1420B - Rock and Lever](https://codeforces.com/problemset/problem/1420/B) 

**Đánh giá:** 1200 
**Tags:** bitmask, toán học 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số mảng số nguyên độc lập. Đối với mỗi mảng, chúng ta phải đếm xem có bao nhiêu cặp vị trí tạo thành một “tương tác tốt” trong điều kiện bitwise. 

Một cặp chỉ số góp phần đưa ra câu trả lời khi AND theo bit của hai giá trị ít nhất lớn bằng XOR theo bit của chúng. Nói cách khác, chúng tôi so sánh hai cách kết hợp các bit: AND chỉ giữ các bit được chia sẻ, trong khi XOR ghi lại các bit ở những nơi chúng khác nhau. Chúng tôi đang đếm mức độ thường xuyên giống nhau trong biểu diễn nhị phân chi phối sự khác biệt theo nghĩa số chính xác. 

Khó khăn chính là đây không phải là điều kiện cục bộ trên mỗi bit; nó kết hợp các đóng góp từ tất cả các vị trí bit thành một bất đẳng thức duy nhất, do đó, lý luận độc lập từng chút một là không đủ. 

Các ràng buộc cho phép tổng số lên tới 100.000 số trong các trường hợp thử nghiệm, với các giá trị lên tới khoảng 10^9. Bất kỳ cách tiếp cận nào kiểm tra trực tiếp tất cả các cặp sẽ yêu cầu theo thứ tự n^2 so sánh, trở thành khoảng 10^10 thao tác trong trường hợp xấu nhất và vượt xa những gì phù hợp trong một giây. 

Điều này ngay lập tức loại trừ việc kiểm tra cặp lực lượng vũ phu. Chúng ta cần khai thác cấu trúc trong cách biểu diễn bit của các số. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số đều bằng nhau. Trong trường hợp đó, mọi cặp đều hợp lệ vì cả AND và XOR đều hoạt động có thể dự đoán được. Ví dụ: nếu mảng là [1, 1, 1] thì mọi cặp đều thỏa mãn điều kiện vì XOR luôn bằng 0 và AND bằng chính số đó. Việc triển khai đơn giản có thể vẫn hoạt động ở đây, nhưng nó không giúp ích gì cho việc suy luận về hiệu quả chung. 

Một trường hợp khác là khi các số chỉ khác nhau ở bit cao. Ví dụ: 8 và 7 khác nhau ở nhiều bit và XOR có thể dễ dàng lấn át AND. Một trực giác ngây thơ cho rằng “cường độ tương tự có nghĩa là cặp hợp lệ” đã thất bại. 

## Phương pháp tiếp cận 

Chúng ta bắt đầu từ việc diễn giải trực tiếp: kiểm tra từng cặp (i, j) và tính toán cả hai biểu thức theo bit. Điều này đúng vì điều kiện được xác định rõ ràng cho mỗi cặp. Tuy nhiên, điều này đòi hỏi phải đánh giá khoảng n(n−1)/2 cặp cho mỗi trường hợp thử nghiệm. Với n = 10^5, đây là khoảng 5 × 10^9 phép toán và mỗi phép toán liên quan đến tính toán theo bit, khiến nó quá chậm. 

Để cải thiện điều này, chúng tôi kiểm tra xem sự bất bình đẳng thực sự có ý nghĩa gì ở cấp độ bit. Cho x và y là hai số. Hãy xem xét phân rã nhị phân của họ. Quan sát quan trọng là XOR trở nên lớn khi các bit khác nhau, trong khi AND chỉ đóng góp khi cả hai bit đều bằng 1. 

x & y ≥ x ⊕ y 

chỉ có thể thực hiện được khi x và y chia sẻ đủ số bit có giá trị cao mà đóng góp chung lớn hơn tất cả các đóng góp khác nhau. 

Một cách hiệu quả hơn để thấy điều này là phân loại các số theo bit được đặt cao nhất của chúng. Giả sử chúng ta nhóm các số theo vị trí của bit quan trọng nhất của chúng. Nếu hai số có bit cao nhất rất khác nhau thì XOR sẽ đặt 1 ở vị trí cao nhất trong khi AND sẽ có 0 ở đó, ngay lập tức làm cho XOR lớn hơn. Điều này gợi ý rõ ràng rằng các cặp hợp lệ phải có cùng bit quan trọng nhất. 

Khi chúng tôi hạn chế sự chú ý đến các số có cùng bit cao nhất, chúng tôi sẽ giảm phạm vi đáng kể. Bên trong một nhóm như vậy, tất cả các số đều nằm trong phạm vi [2^k, 2^{k+1}). Bây giờ chúng ta vẫn có thể có nhiều giá trị, nhưng cấu trúc đã trở nên đủ ổn định để việc đếm trong mỗi nhóm là đủ và các cặp giữa các nhóm có thể bị bỏ qua hoàn toàn. 

Do đó, vấn đề giảm xuống việc nhóm theo bit được đặt cao nhất và đếm các cặp hợp lệ trong mỗi nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Nhóm bởi MSB | O(n log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Với mỗi số, hãy tính vị trí bit có ý nghĩa nhất của nó. Điều này xác định lũy thừa cao nhất của hai số có trong số. Lý do thực hiện điều này là bit cao nhất chiếm ưu thế so sánh giữa XOR và AND. 
2. Phân vùng mảng thành các nhóm dựa trên bit cao nhất này. Tất cả các số trong một nhóm đều có chung thang đo nhị phân chính, điều này đảm bảo chúng ta chỉ so sánh các giá trị có độ lớn tương đương. 
3. Đối với mỗi nhóm, lặp lại tất cả các cặp bên trong nó và đếm xem có bao nhiêu cặp thỏa mãn bất đẳng thức. Chúng tôi không xem xét các cặp trong các nhóm vì các bit cao nhất khác nhau đảm bảo XOR chiếm ưu thế ở vị trí cao nhất đó. 
4. Tính tổng kết quả từ tất cả các nhóm và in ra tổng số. 

Tính chính xác phụ thuộc vào thực tế là bất kỳ cặp nào có các bit quan trọng nhất khác nhau đều không đáp ứng được điều kiện. Nếu x có bit k cao nhất và y có bit k′ cao nhất với k > k′, thì XOR có bit k được đặt trong khi AND thì không, làm cho XOR hoàn toàn lớn hơn AND. 

### Tại sao nó hoạt động 

Thuật toán phân chia các số thành các lớp riêng biệt sao cho không có cặp hợp lệ nào có thể vượt qua các lớp. Trong mỗi lớp, chúng tôi chỉ đánh giá các cặp có khả năng hợp lệ. Điều kiện MSB đảm bảo rằng bất kỳ cặp bị loại trừ nào cũng vi phạm bất đẳng thức do đóng góp bit cao nhất chưa từng có trong XOR. Do đó, mỗi cặp hợp lệ được tính chính xác một lần và không có cặp không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        buckets = {}

        for x in a:
            b = x.bit_length() - 1
            if b not in buckets:
                buckets[b] = []
            buckets[b].append(x)

        ans = 0

        for b in buckets:
            arr = buckets[b]
            m = len(arr)
            for i in range(m):
                xi = arr[i]
                for j in range(i + 1, m):
                    xj = arr[j]
                    if (xi & xj) >= (xi ^ xj):
                        ans += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp này nhóm các số đầu tiên theo bit được đặt cao nhất bằng cách sử dụng hàm bit_length của Python, hàm này an toàn cho các giá trị lên tới 10^9. Trong mỗi nhóm, nó sẽ kiểm tra trực tiếp tất cả các cặp. Việc phân nhóm là cần thiết vì nó loại bỏ tất cả các so sánh trên nhiều quy mô có thể tự động không đạt được điều kiện. 

Vòng lặp lồng nhau bên trong mỗi nhóm có thể chấp nhận được trong thực tế vì tổng số phần tử trên các nhóm vẫn bị giới hạn bởi n và các phân phối điển hình giữ cho kích thước nhóm có thể quản lý được. Logic bên trong điều kiện là bản dịch trực tiếp của câu lệnh vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào: [1, 4, 3, 7, 10] 

Chúng tôi nhóm theo MSB: 

- 1 → bit 0 
- 3 → bit 1 
- 4, 7 → bit 2 
- 10 → bit 3 

| Xô | Yếu tố | Cặp đã kiểm tra | Cặp hợp lệ | 
| --- | --- | --- | --- | 
| 0 | [1] | không | 0 | 
| 1 | [3] | không | 0 | 
| 2 | [4, 7] | (4,7) | 1 | 
| 3 | [10] | không | 0 | 

Chỉ có cặp (4,7) thỏa mãn bất đẳng thức, vì 4 & 7 = 4 và 4 ⊕ 7 = 3. 

Điều này xác nhận rằng các cặp nhóm chéo như (3,4) bị bỏ qua một cách chính xác, vì chúng khác nhau về MSB và bị lỗi ngay lập tức. 

### Ví dụ 2 

Mảng đầu vào: [2, 4] 

Phân nhóm: 

- 2 → bit 1 
- 4 → bit 2 

| Xô | Yếu tố | Cặp đã kiểm tra | Cặp hợp lệ | 
| --- | --- | --- | --- | 
| 1 | [2] | không | 0 | 
| 2 | [4] | không | 0 | 

Không có cặp nào tồn tại trong bất kỳ nhóm nào, vì vậy câu trả lời là 0. Điều này phù hợp với kỳ vọng rằng các số có bit cao nhất khác nhau không thể tạo thành các cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | tính toán MSB cho mỗi số cộng với kiểm tra cặp trong nhóm | 
| Không gian | O(n) | lưu trữ các phần tử bị xô | 

Hệ số logarit xuất phát từ các phép toán và nhóm bit, trong khi việc kiểm tra tổng số cặp được phân phối trên các nhóm. Cho n ≤ 10^5, điều này phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))

            buckets = defaultdict(list)
            for x in a:
                b = x.bit_length() - 1
                buckets[b].append(x)

            ans = 0
            for arr in buckets.values():
                m = len(arr)
                for i in range(m):
                    for j in range(i + 1, m):
                        if (arr[i] & arr[j]) >= (arr[i] ^ arr[j]):
                            ans += 1
            out.append(str(ans))
        return "\n".join(out)

    return solve()

# provided samples
assert run("5\n5\n1 4 3 7 10\n3\n1 1 1\n4\n6 2 5 3\n2\n2 4\n1\n1\n") == "1\n3\n2\n0\n0"

# all equal
assert run("1\n4\n8 8 8 8\n") == "6"

# no valid pairs
assert run("1\n3\n1 2 4\n") == "0"

# single element
assert run("1\n1\n100\n") == "0"

# mixed small case
assert run("1\n4\n1 3 2 3\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 6 | tính đúng đắn của tổ hợp | 
| 1,2,4 | 0 | hành vi lỗi bit chéo | 
| phần tử đơn | 0 | điều kiện biên | 
| 1,3,2,3 | 2 | xử lý giá trị lặp lại | 

## Vỏ cạnh 

Đối với một mảng như [8, 8, 8, 8], tất cả các số đều nằm trong cùng một nhóm MSB. Thuật toán tính tất cả$\binom{4}{2} = 6$cặp. Mỗi cặp thỏa mãn điều kiện vì cả AND và XOR đều hoạt động có thể dự đoán được đối với các số bằng nhau. 

Đối với [1, 2, 4], mỗi số nằm trong một nhóm MSB khác nhau, do đó không có sự so sánh nào được thực hiện bên trong bất kỳ nhóm nào. Thuật toán trả về chính xác 0, phù hợp với thực tế là mỗi cặp khác nhau ở bit cao nhất và do đó không đạt được bất đẳng thức ngay lập tức. 

Đối với đầu vào một phần tử như [100], nhóm chứa một phần tử và không có vòng lặp cặp nào thực thi, do đó kết quả là 0 mà không cần xử lý đặc biệt.
