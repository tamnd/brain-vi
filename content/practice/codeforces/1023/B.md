---
title: "CF 1023B - Cặp đồ chơi"
description: "Chúng ta được cho một dãy giá đồ chơi hoàn toàn đều đặn: có những đồ chơi có giá từ 1 đến n, mỗi giá nguyên xuất hiện đúng một lần. Chúng ta muốn đếm có bao nhiêu cặp đồ chơi khác nhau không có thứ tự riêng biệt có giá tổng hợp chính xác bằng k."
date: "2026-06-16T21:52:28+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1023
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 504 (rated, Div. 1 + Div. 2, based on VK Cup 2018 Final)"
rating: 1000
weight: 1023
solve_time_s: 112
verified: true
draft: false
---

[CF 1023B - Cặp đồ chơi](https://codeforces.com/problemset/problem/1023/B) 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy giá đồ chơi hoàn toàn đều đặn: có những đồ chơi có giá từ 1 đến n, mỗi giá nguyên xuất hiện đúng một lần. Chúng ta muốn đếm có bao nhiêu cặp đồ chơi khác nhau không có thứ tự riêng biệt có giá tổng hợp chính xác bằng k. Mỗi cặp được xác định bởi hai số nguyên a và b phân biệt với 1 ≤ a < b ≤ n và a + b = k. 

Đầu ra chỉ đơn giản là số lượng các cặp hợp lệ như vậy. 

Các ràng buộc rất cao: cả n và k đều có thể lớn tới 10^14. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp đi lặp lại trên nhiều loại đồ chơi. Quét tuyến tính lên tới n sẽ yêu cầu tới 10^14 thao tác, vượt xa mọi giới hạn thời gian khả thi. Ngay cả các cách tiếp cận logarit hoặc đa thức-in-n cũng không cần thiết; cấu trúc của bài toán gợi ý rằng chúng ta nên tránh lặp lại hoàn toàn trên miền và thay vào đó suy luận đại số về các cặp hợp lệ. 

Trường hợp thất bại tinh tế phổ biến nhất ở đây xuất phát từ việc xử lý ranh giới. Một nỗ lực ngây thơ có thể tính toán một đối tác hợp lệ b = k - a và đếm tất cả a trong [1, n] sao cho b cũng nằm trong [1, n]. Sai lầm xuất hiện khi quên rằng các cặp phải thỏa mãn a < b, nếu không các cặp sẽ được tính gấp đôi. Một lỗi phổ biến khác xảy ra khi cho phép a = b, lỗi này chỉ xảy ra khi k chẵn và a = k/2. Điều này phải được loại trừ ngay cả khi nó nằm trong phạm vi. 

Ví dụ: nếu n = 8 và k = 5, các cặp hợp lệ là (1, 4) và (2, 3). Việc triển khai bất cẩn đếm tất cả a hợp lệ mà không thực thi a < b sẽ tính cả (1, 4) và (4, 1), nhân đôi câu trả lời. Tương tự, nếu n = 10 và k = 10, cặp (5, 5) sẽ được đưa vào không chính xác trừ khi bị loại trừ rõ ràng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi đồ chơi đầu tiên a có thể từ 1 đến n, tính b = k - a và kiểm tra xem b có nằm trong [a + 1, n hay không). Điều này đúng vì nó trực tiếp thực thi tất cả các ràng buộc. Tuy nhiên, nó yêu cầu lặp lại tất cả n giá trị, dẫn đến độ phức tạp về thời gian O(n), điều này trở nên không thể thực hiện được khi n lên đến 10^14. 

Quan sát quan trọng là khi a cố định thì b được xác định hoàn toàn, do đó, vấn đề thực sự là đếm các số nguyên hợp lệ a thỏa mãn đồng thời hai bất đẳng thức: b = k - a phải nằm trong [1, n], và a < b cũng phải thỏa mãn. Những điều kiện này chuyển bài toán thành các khoảng nguyên giao nhau. 

Từ b = k - a, ràng buộc 1 ≤ b ≤ n trở thành 1 ≤ k - a ≤ n, sắp xếp lại thành k - n ≤ a ≤ k - 1. Ràng buộc a < b trở thành a < k - a, hoặc tương đương 2a < k, nghĩa là a ≤ (k - 1) // 2. Cuối cùng, chúng ta đã có 1 ≤ a ≤ n từ định nghĩa đồ chơi hợp lệ. 

Vậy a phải nằm trên giao điểm của ba khoảng: 

1  a  n 

k - n `a `` k - 1 

1 ≤ a ≤ (k - 1) // 2 

Khi đã biết giao điểm này, câu trả lời chỉ đơn giản là số số nguyên trong đó, tối đa (0, R - L + 1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giới hạn dưới của các giá trị a hợp lệ là L = max(1, k - n). Điều này đảm bảo rằng b = k - a không vượt quá n. 
2. Tính giới hạn trên của các giá trị a hợp lệ là R = min(n, k - 1). Điều này đảm bảo b ít nhất là 1. 
3. Thực thi điều kiện phân biệt a < b bằng cách thắt chặt giới hạn trên thành R = min(R, (k - 1) // 2). 
4. Nếu L > R, không có số nguyên hợp lệ a thỏa mãn mọi ràng buộc, vì vậy câu trả lời là 0. 
5. Ngược lại, số a hợp lệ là R - L + 1. 

Lý do các bước này được sắp xếp theo cách này là vì mỗi ràng buộc hạn chế một cách độc lập các giá trị có thể có của a. Việc lấy giao điểm của tất cả các khoảng hợp lệ đảm bảo chúng ta đếm chính xác những cặp thỏa mãn tất cả các điều kiện cùng một lúc. 

### Tại sao nó hoạt động

Mỗi cặp hợp lệ (a, b) tương ứng với chính xác một số nguyên a và a phải thỏa mãn tất cả các bất đẳng thức dẫn xuất. Ngược lại, mọi số nguyên a trong khoảng cuối cùng tạo ra một b = k - a hợp lệ duy nhất nằm trong phạm vi và lớn hơn a. Việc chuyển đổi làm giảm vấn đề đếm tổ hợp ban đầu thành việc đếm các điểm nguyên trong một khoảng duy nhất và không có cấu hình hợp lệ nào bị mất hoặc trùng lặp trong ánh xạ này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

L = max(1, k - n)
R = min(n, k - 1)
R = min(R, (k - 1) // 2)

ans = max(0, R - L + 1)
print(ans)
```Mã trực tiếp thực hiện giao điểm khoảng xuất phát trong thuật toán. Giới hạn đầu tiên buộc đồ chơi thứ hai tồn tại trong mức giá tối đa n. Điều thứ hai đảm bảo tính tích cực và a và b khác biệt về mặt thứ tự. Lệnh thứ ba thực thi a < b, loại bỏ các trường hợp trùng lặp đối xứng và các trường hợp điểm giữa không hợp lệ. 

Một chi tiết triển khai tinh tế là thứ tự áp dụng các ràng buộc: mặc dù giao điểm có tính giao hoán về mặt toán học, việc áp dụng ràng buộc đối xứng cuối cùng sẽ tránh vô tình đếm các giá trị trung điểm không hợp lệ, chẳng hạn như k/2 khi k chẵn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: n = 8, k = 5 

| Bước | L | R (trước tính đối xứng) | R (sau đối xứng) | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 4 | 2 | 

Chúng tôi tính L = max(1, 5 - 8) = 1. R bắt đầu là min(8, 4) = 4. Sau đó, chúng tôi thực thi tính đối xứng: (k - 1) // 2 = 2, do đó R trở thành 2. Giá trị a hợp lệ cuối cùng là {1, 2}, tạo ra các cặp (1, 4) và (2, 3). 

Điều này xác nhận rằng phương pháp khoảng tránh tính chính xác các cặp đảo ngược hoặc không hợp lệ trong khi vẫn thu được tất cả các cặp hợp lệ. 

### Ví dụ 2 

Đầu vào: n = 10, k = 10 

| Bước | L | R (trước tính đối xứng) | R (sau đối xứng) | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 9 | 4 | 

Ở đây L = max(1, 10 - 10) = 1. R trước đối xứng là min(10, 9) = 9. Sau khi áp dụng đối xứng, R = 4. Các giá trị a hợp lệ là {1, 2, 3, 4}, tương ứng với các cặp (1,9), (2,8), (3,7), (4,6). Điểm giữa không hợp lệ (5,5) sẽ tự động bị loại trừ. 

Điều này chứng tỏ điều kiện a < b được thực thi hoàn toàn thông qua giới hạn thay vì kiểm tra rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép tính số học và đánh giá tối thiểu/tối đa được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp thời gian không đổi dễ dàng thỏa mãn các ràng buộc ngay cả khi n và k lên tới 10^14, vì nó tránh hoàn toàn việc lặp lại và giảm bài toán về số học khoảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import *
    n, k = map(int, sys.stdin.readline().split())

    L = max(1, k - n)
    R = min(n, k - 1)
    R = min(R, (k - 1) // 2)

    return str(max(0, R - L + 1))

# provided samples
assert run("8 5\n") == "2"

# k too small
assert run("10 1\n") == "0"

# only one valid pair
assert run("10 19\n") == "1"

# midpoint excluded case
assert run("10 10\n") == "4"

# large symmetric case
assert run("1000000000000 1000000000001\n") == "500000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 8 5 | 2 | trường hợp hai nghiệm cơ bản | 
| 10 1 | 0 | không có cặp hợp lệ | 
| 10 19 | 1 | cặp ranh giới đơn | 
| 10 10 | 4 | tính đúng đắn của việc loại trừ điểm giữa | 
| 1e12 1e12+1 | 5e11 | hiệu quả biên lớn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi k nhỏ, cụ thể là k ≤ 2. Trong trường hợp này, k - n luôn ≤ 0, do đó L trở thành 1, nhưng R trở thành min(n, k - 1), tối đa là 1. Nếu k = 1 hoặc k = 2, R ≤ 1 và sau khi thực thi a < b, R trở thành 0, tạo ra một khoảng trống. Thuật toán trả về đúng 0. 

Một trường hợp quan trọng khác là khi k rất lớn so với n, ví dụ n = 5 và k = 100. Khi đó k - n = 95, do đó L = 95 trong khi R = min(5, 99) = 5. Vì L > R, khoảng trống, cho kết quả chính xác là 0. Điều này phản ánh rằng không có hai số nào trong [1, n] có thể tổng bằng một k lớn như vậy. 

Trường hợp tinh tế cuối cùng là khi k chẵn và k/2 nằm trong [1, n]. Ví dụ n = 10, k = 10 cho điểm giữa là 5. Việc xây dựng khoảng ban đầu sẽ bao gồm a = 5, nhưng việc thực thi R ≤ (k - 1) // 2 sẽ loại bỏ nó. Điều này đảm bảo chúng tôi không bao giờ tính số tự ghép không hợp lệ (5, 5), duy trì tính chính xác mà không cần phân nhánh trong trường hợp đặc biệt.
