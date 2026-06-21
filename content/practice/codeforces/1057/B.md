---
title: "CF 1057B - DDoS"
description: "Chúng tôi được cung cấp một dòng thời gian được chia thành vài giây. Trong mỗi giây, chúng tôi biết có bao nhiêu yêu cầu gửi đến máy chủ. Điều này cung cấp cho chúng ta một mảng trong đó mỗi vị trí biểu thị khối lượng yêu cầu trong giây đó."
date: "2026-06-15T13:06:02+07:00"
tags: ["codeforces", "competitive-programming", "*special", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1057
codeforces_index: "B"
codeforces_contest_name: "Mail.Ru Cup 2018 - Practice Round"
rating: 1400
weight: 1057
solve_time_s: 265
verified: true
draft: false
---

[CF 1057B - DDoS](https://codeforces.com/problemset/problem/1057/B) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, vũ phu 
**Thời gian giải:** 4 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng thời gian được chia thành vài giây. Trong mỗi giây, chúng tôi biết có bao nhiêu yêu cầu gửi đến máy chủ. Điều này cung cấp cho chúng ta một mảng trong đó mỗi vị trí biểu thị khối lượng yêu cầu trong giây đó. 

Chúng ta cần tìm khoảng thời gian liền kề dài nhất sao cho tổng số yêu cầu trong khoảng đó lớn hơn 100 lần độ dài của khoảng. Nói cách khác, nếu chúng ta chọn một đoạn có độ dài`t`và tổng số yêu cầu trong đó vượt quá`100 * t`, đoạn đó được coi là giai đoạn tấn công và chúng tôi muốn độ dài tối đa có thể có của đoạn đó. 

Kích thước đầu vào lên tới 5000 giây và mỗi giá trị cũng có thể lên tới 5000. Điều này ngay lập tức gợi ý rằng giải pháp O(n^2) có thể chấp nhận được vì n^2 là khoảng 25 triệu thao tác, vừa vặn thoải mái trong 2 giây bằng Python với tổng tiền tố. 

Một quan sát quan trọng là điều kiện chỉ phụ thuộc vào tổng theo các khoảng. Điều này làm cho việc tính tổng tiền tố trở thành một công cụ tự nhiên vì nó cho phép truy vấn tính tổng theo phạm vi thời gian không đổi. 

Khó khăn chính không phải là tính tổng mà là kiểm tra hiệu quả tất cả các khoảng ứng cử viên và trích xuất độ dài hợp lệ tối đa. 

Trường hợp cạnh tinh tế phát sinh khi không có khoảng nào thỏa mãn điều kiện. Ví dụ: nếu tất cả các giá trị đều bằng 0 thì mọi khoảng đều có tổng bằng 0, không bao giờ lớn hơn`100 * t`. Trong trường hợp này, câu trả lời phải là 0, không phải số âm hoặc -1. 

Một trường hợp cạnh khác là khi chỉ có các phần tử duy nhất đủ điều kiện. Ví dụ, nếu`r[i] = 101`, thì khoảng thời gian có độ dài-1 là hợp lệ, nhưng khoảng thời gian dài hơn có thể không thành công. Giải pháp vẫn phải phát hiện và so sánh chính xác tất cả các độ dài hợp lệ. 

Cuối cùng, lưu ý rằng điều kiện là nghiêm ngặt:`sum > 100 * length`. sử dụng`>=`thay vào đó sẽ bao gồm không chính xác các trường hợp ranh giới như chính xác`100 * t`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ kiểm tra mọi mảng con có thể có. Đối với mỗi chỉ số bắt đầu`i`, chúng tôi mở rộng đến mọi chỉ số kết thúc`j`, tính tổng của`r[i..j]`, và kiểm tra xem nó có vượt quá`100 * (j - i + 1)`. Điều này đúng vì nó đánh giá rõ ràng mọi khoảng thời gian có thể. 

Tuy nhiên, việc tính lại tổng từ đầu cho mỗi khoảng sẽ dẫn đến độ phức tạp O(n^3) nếu thực hiện một cách ngây thơ. Ngay cả khi sử dụng lại một phần, một vòng lặp kép với các cập nhật tổng tăng dần sẽ mang lại O(n^2), đây là kết quả tốt nhất mà chúng ta có thể hy vọng ở dạng bạo lực. 

Cải tiến quan trọng là tính toán trước các tổng tiền tố để bất kỳ tổng khoảng nào cũng có thể được tính theo O(1). Điều này làm giảm việc kiểm tra cho mỗi cặp`(i, j)`đến thời gian không đổi. Chúng tôi vẫn liệt kê tất cả các cặp, nhưng bây giờ mỗi tờ séc đều rẻ. 

Cấu trúc của bài toán không cho phép tốt hơn O(n^2) vì chúng ta được yêu cầu rõ ràng về khoảng thời gian hợp lệ dài nhất mà không có ràng buộc đơn điệu cho phép tìm kiếm hai con trỏ hoặc tìm kiếm nhị phân. Điều kiện không đơn điệu về độ dài khoảng nên không áp dụng được kỹ thuật cửa sổ trượt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (lồng nhau + tính toán lại) | O(n^3) | O(1) | Quá chậm | 
| Tiền tố Tổng + O(n^2) liệt kê | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng tổng tiền tố trong đó`pref[i]`lưu trữ tổng của số đầu tiên`i`các phần tử. Điều này cho phép tính toán nhanh bất kỳ tổng phân đoạn nào sau này. 
2. Lặp lại tất cả các điểm cuối có thể có ở bên trái`l`từ 0 đến n - 1. 
3. Đối với mỗi`l`, mở rộng điểm cuối bên phải`r`từ`l`đến n-1. 
4. Tính tổng của đoạn`[l, r]`sử dụng`pref[r+1] - pref[l]`. 
5. Tính độ dài của đoạn đó là`r - l + 1`. 
6. Kiểm tra xem`segment_sum > 100 * length`. 
7. Nếu điều kiện đúng, hãy cập nhật câu trả lời với độ dài tối đa được thấy cho đến nay. 

Lý do chúng tôi quét tất cả`(l, r)`cặp là bất kỳ khoảng hợp lệ nào cũng phải xuất hiện ở đâu đó trong bảng liệt kê này. Tổng tiền tố đảm bảo rằng chúng tôi không tính toán lại các tổng chồng chéo nhiều lần. 

### Tại sao nó hoạt động 

Thuật toán đánh giá mọi khoảng thời gian liền kề có thể chính xác một lần. Đối với mỗi khoảng, tính chính xác của việc kiểm tra chỉ phụ thuộc vào sự so sánh số học chính xác giữa tổng của nó và ngưỡng tuyến tính dựa trên độ dài của nó. Vì tổng tiền tố đưa ra tổng khoảng chính xác nên không liên quan đến phép tính gần đúng hoặc phương pháp phỏng đoán. Do đó, bất kỳ khoảng thời gian nào thỏa mãn điều kiện sẽ được phát hiện và độ dài tối đa giữa chúng sẽ được theo dõi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    r = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + r[i]

    ans = 0

    for l in range(n):
        for rgt in range(l, n):
            total = pref[rgt + 1] - pref[l]
            length = rgt - l + 1
            if total > 100 * length:
                if length > ans:
                    ans = length

    print(ans)

if __name__ == "__main__":
    solve()
```
