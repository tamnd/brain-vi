---
title: "CF 104295C - \u0420\u0430\u043a\u0443\u0448\u043a\u0438 \u041c\u0443\u043c\u0438-\u043c\u0430\u043c\u044b"
description: "Chúng tôi được cung cấp một danh sách các luống hoa, mỗi luống hoa được liên kết với một số vỏ sò. Đối với chiếc giường thứ i, có những chiếc vỏ ai phải được sử dụng để tạo thành đường viền trang trí."
date: "2026-07-01T20:18:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104295
codeforces_index: "C"
codeforces_contest_name: "vkoshp.letovo"
rating: 0
weight: 104295
solve_time_s: 53
verified: true
draft: false
---

[CF 104295C - \u0420\u0430\u043a\u0443\u0448\u043a\u0438 \u041c\u0443\u043c\u0438-\u043c\u0430\u043c\u044b](https://codeforces.com/problemset/problem/104295/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các luống hoa, mỗi luống hoa được liên kết với một số vỏ sò. Đối với chiếc giường thứ i, có những chiếc vỏ ai phải được sử dụng để tạo thành đường viền trang trí. 

Quy tắc trang trí mang tính hình học nhưng giảm xuống một ràng buộc tổ hợp rõ ràng: chúng tôi muốn sắp xếp tất cả các lớp vỏ ai thành một đa giác đều trong đó mỗi cạnh của đa giác có cùng số lượng lớp vỏ. Mỗi chiếc vỏ phải nằm ở một phía nào đó và không có chiếc vỏ nào được sử dụng. Trong số tất cả các đa giác hợp lệ có thể có cho một ai nhất định, chúng ta phải chọn đa giác có số cạnh nhỏ nhất. 

Vì vậy, đối với mỗi ai, chúng tôi đang tìm cách chia ai thành k phần bằng nhau một cách hiệu quả, trong đó k là số cạnh và mỗi bên chứa các vỏ ai / k. Điều này có nghĩa là k phải là ước của ai. Nhiệm vụ trở thành: với mỗi ai, tìm ước số k lớn nhất có thể ít nhất là 3, vì một đa giác phải có ít nhất ba cạnh. 

Các ràng buộc cho phép n lên tới 200000 và ai lên tới 10^7. Điều này ngay lập tức loại trừ việc kiểm tra tất cả các giá trị tối đa ai cho mọi truy vấn, vì điều đó sẽ quá chậm. Ngay cả một O(ai) đơn giản cho mỗi truy vấn cũng sẽ dẫn đến khoảng 2 * 10^12 thao tác trong trường hợp xấu nhất, điều này là không thể trong vòng một giây. Do đó, chúng tôi cần giảm từng truy vấn xuống gần hơn với O(sqrt(ai)) hoặc tốt hơn. 

Một trường hợp góc tinh tế phát sinh khi ai là số nguyên tố. Ví dụ: nếu ai = 17 thì ước số duy nhất là 1 và 17, và vì 17 hợp lệ và ≥ 3 nên câu trả lời là 17, tương ứng với một đa giác suy biến trong đó mỗi cạnh có một vỏ. Một trường hợp khác là khi ai có ước số nhỏ nhưng không có ước số nào đủ lớn để tối đa hóa k. Ví dụ, ai = 18 có các ước số 2, 3, 6, 9, 18 và k tốt nhất là 18, vì nó cho các cạnh có kích thước 1. Một cách tiếp cận ngây thơ thay vì giảm thiểu độ dài cạnh thay vì tối đa hóa số cạnh sẽ chọn k = 9 hoặc k = 6 không chính xác tùy thuộc vào lỗi diễn giải. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Với mỗi ai, chúng ta thử tất cả k từ 3 đến ai và kiểm tra xem ai % k == 0. Trong số k hợp lệ, chúng ta chọn giá trị lớn nhất. Điều này đúng vì mọi cấu hình đa giác hợp lệ đều tương ứng chính xác với ước số của ai và việc tối đa hóa k sẽ giảm thiểu số lượng vỏ mỗi cạnh. 

Vấn đề là hiệu suất. Trong trường hợp xấu nhất, ai là khoảng 10^7, do đó, việc kiểm tra tất cả k trên mỗi truy vấn sẽ dẫn đến khoảng 10^7 thao tác cho mỗi phần tử và với tối đa 2 * 10^5 phần tử, điều này hoàn toàn không khả thi. 

Điều quan trọng là chúng ta không cần phải quét tất cả k. Chúng ta chỉ cần tìm ước số lớn nhất của ai. Thay vì tìm kiếm lên trên, chúng ta tìm kiếm đi xuống từ sqrt(ai), vì mỗi cặp ước số (d, ai/d) đều có một phần tử ∼ sqrt(ai) và một ≥ sqrt(ai). Điều này cho phép chúng ta phát hiện ứng cử viên tốt nhất một cách nhanh chóng: bất cứ khi nào chúng ta tìm thấy ước số d, chúng ta có thể ngay lập tức coi ai / d là một câu trả lời ứng cử viên lớn. 

Do đó, với mỗi ai, chúng ta lặp d từ 1 đến sqrt(ai), kiểm tra tính chia hết và theo dõi ước số hợp lệ lớn nhất k, xem xét cả d và ai / d khi chúng ít nhất bằng 3. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · a_i) | O(1) | Quá chậm | 
| Quét căn bậc hai | O(n √a_i) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Với mỗi giá trị ai, chúng ta bắt đầu bằng việc giả sử câu trả lời chính là ai. Điều này hợp lệ vì mỗi số đều tự chia hết và luôn có ít nhất 3 ràng buộc cho trước. 
2. Chúng ta lặp lại tất cả các số nguyên d từ 1 đến ⌊√ai⌋. Điều này có hiệu quả vì bất kỳ ước số nào lớn hơn √ai đều phải được ghép với một ước số nhỏ hơn đã có trong phạm vi này. 
3. Nếu d chia ai thì ta tính ước số ghép x = ai / d. Tại thời điểm này, cả d và x đều là ước số hợp lệ của ai. 
4. Chúng ta coi d là câu trả lời ứng viên nếu d ≥ 3, vì một đa giác phải có ít nhất 3 cạnh. Chúng tôi cập nhật câu trả lời thành max(answer, d). Bước này đảm bảo chúng tôi nắm bắt được các ước số nhỏ nhưng có khả năng phù hợp. 
5. Chúng ta cũng coi x là câu trả lời ứng viên nếu x ≥ 3 và cập nhật câu trả lời tương tự. Điều này thu được các ước số lớn, có giá trị hơn vì chúng tương ứng với độ dài các cạnh nhỏ hơn và do đó cấu trúc đa giác tốt hơn theo sở thích của bài toán. 
6. Sau khi kết thúc vòng lặp, chúng ta đưa ra ước số tốt nhất tìm được. 

Tại sao nó hoạt động phụ thuộc vào cấu trúc của các ước số. Mọi cặp ước số (d, ai / d) đều được bao phủ hoàn toàn khi chúng ta chỉ lặp lại tối đa √ai. Vì chúng ta đánh giá cả hai thành viên của mỗi cặp nên không có ước số ứng cử viên nào bị bỏ sót. Thuật toán duy trì bất biến rằng sau khi xử lý tất cả d cho đến thời điểm hiện tại, câu trả lời được lưu trữ là ước số lớn nhất của ai gặp phải cho đến nay, ít nhất là 3. Khi vòng lặp kết thúc, tất cả các cặp ước số có thể có đã được xem xét, do đó, bất biến đảm bảo câu trả lời cuối cùng là ước số hợp lệ lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    res = []
    
    for a in arr:
        best = a
        
        d = 1
        while d * d <= a:
            if a % d == 0:
                x = a // d
                if d >= 3:
                    best = max(best, d)
                if x >= 3:
                    best = max(best, x)
            d += 1
        
        res.append(str(best))
    
    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo lý luận cặp chia. Chúng tôi khởi tạo tốt nhất dưới dạng chính nó để xử lý các trường hợp như số nguyên tố không tồn tại ước số nhỏ hơn ≥ 3. Vòng lặp chỉ chạy đến căn bậc hai, đảm bảo hiệu quả. 

Một lỗi thường gặp là quên kiểm tra cả d và a // d. Một vấn đề tế nhị khác là bắt đầu câu trả lời từ 0 hoặc 1, điều này sẽ cho phép chọn sai số cạnh đa giác không hợp lệ dưới 3 nếu không được lọc cẩn thận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [10]
```Chúng tôi xử lý 10. 

| d | d chia 10 | x = 10/ngày | ứng viên | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | vâng | 10 | 10 | 10 | 
| 2 | vâng | 5 | 10, 5 | 10 | 
| 3 | không | - | - | 10 | 

Câu trả lời cuối cùng là 10. 

Điều này cho thấy sự lựa chọn tối ưu là ước số lớn nhất chứ không phải độ dài cạnh nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
a = [18]
```| d | d chia 18 | x = 18/ngày | ứng viên | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | vâng | 18 | 18 | 18 | 
| 2 | vâng | 9 | 18, 9 | 18 | 
| 3 | vâng | 6 | 18, 9, 6 | 18 | 

Câu trả lời cuối cùng là 18. 

Điều này xác nhận rằng ngay cả khi tồn tại nhiều cấu hình đa giác hợp lệ, thuật toán luôn ưu tiên cấu hình có số cạnh tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n √a_i) | Mỗi số được phân tích thành thừa số bằng cách quét đến căn bậc hai của nó | 
| Không gian | O(1) | Chỉ sử dụng các biến phụ không đổi | 

Các ràng buộc cho phép tối đa 200000 số có giá trị lên tới 10^7 và √10^7 là khoảng 3162. Điều này dẫn đến khoảng 6 * 10^8 kiểm tra nguyên thủy trong trường hợp xấu nhất, có thể chấp nhận được trong Python được tối ưu hóa theo các ràng buộc CF điển hình, đặc biệt vì hầu hết các số sẽ chấm dứt sớm hơn do các yếu tố nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input())
    arr = list(map(int, input().split()))
    
    res = []
    for a in arr:
        best = a
        d = 1
        while d * d <= a:
            if a % d == 0:
                x = a // d
                if d >= 3:
                    best = max(best, d)
                if x >= 3:
                    best = max(best, x)
            d += 1
        res.append(str(best))
    
    return " ".join(res)

# provided sample
assert run("5\n10 17 18 19 10") == "10 17 18 19 10"

# minimum size (smallest valid polygon)
assert run("1\n3") == "3"

# prime behavior
assert run("3\n3 5 17") == "3 5 17"

# perfect square behavior
assert run("1\n36") == "36"

# composite rich divisor structure
assert run("1\n100") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | 3 | trường hợp đa giác hợp lệ tối thiểu | 
| số nguyên tố | cùng số | không có ước số ≥ 3 ngoại trừ chính nó | 
| 36 | 36 | nhiều cặp số chia được xử lý chính xác | 
| 100 | 100 | chọn đúng giữa nhiều ước | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi ai là số nguyên tố, chẳng hạn như 17. Vòng lặp kiểm tra d từ 1 đến 4. Chỉ d = 1 chia hết 17, tạo ra ứng cử viên 17. Vì không có ước số nào khác tồn tại nên tốt nhất vẫn là 17, kết quả này đúng. 

Một trường hợp khác là khi ai có nhiều ước nhỏ nhưng đáp số tối ưu lại lớn. Với ai = 30 thì các ước gồm 2, 3, 5, 6, 10, 15, 30. Thuật toán sẽ gặp 1, 2, 3, 5. Mỗi cặp hợp lệ đóng góp cả hai phần tử và 30 luôn có sẵn từ d = 1 nên vẫn là giá trị lớn nhất. 

Cuối cùng, đối với các ô vuông hoàn hảo như 36, các cặp ước số trùng nhau ở d = 6. Điều kiện vẫn xử lý chính xác cả hai vế của cặp một lần, đảm bảo không có sự trùng lặp ảnh hưởng đến độ chính xác trong khi vẫn thu được ước số tối đa 36.
