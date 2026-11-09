---
title: "CF 1462B - Chuỗi con của năm ngoái"
description: "Vấn đề trình bày một chuỗi chữ số và hỏi liệu nó có thể được chuyển đổi thành chuỗi \"2020\" bằng cách sử dụng tối đa một lần xóa liền kề hay không. Polycarp có thể loại bỏ 0 hoặc nhiều ký tự liên tiếp ở bất kỳ đâu trong chuỗi."
date: "2026-06-11T02:09:17+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1462
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 690 (Div. 3)"
rating: 800
weight: 1462
solve_time_s: 211
verified: true
draft: false
---

[CF 1462B - Chuỗi con của năm ngoái](https://codeforces.com/problemset/problem/1462/B) 

**Đánh giá:** 800 
**Tags:** dp, triển khai, chuỗi 
**Thời gian giải:** 3 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán trình bày một chuỗi chữ số và hỏi liệu nó có thể được chuyển đổi thành chuỗi "2020" bằng cách sử dụng tối đa một lần xóa liền kề hay không. Polycarp có thể loại bỏ 0 hoặc nhiều ký tự liên tiếp ở bất kỳ đâu trong chuỗi. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp độ dài của chuỗi và chính chuỗi đó. Đầu ra cho mỗi trường hợp thử nghiệm là "CÓ" nếu có thể chuyển đổi như vậy và "KHÔNG" nếu ngược lại. 

Các ràng buộc ở mức vừa phải: các chuỗi có độ dài từ 4 đến 200 và có thể có tới 1000 trường hợp kiểm thử. Vì mỗi chuỗi tương đối ngắn nên một giải pháp có thể đủ khả năng kiểm tra nhiều lần xóa có thể xảy ra mà không vượt quá giới hạn thời gian. Các trường hợp cạnh bao gồm các chuỗi chính xác là "2020", các chuỗi có "2020" ở đầu hoặc cuối và các chuỗi trong đó "2020" có thể được hình thành bằng cách xóa khối bên trong. Ví dụ: "22020" có thể trở thành "2020" bằng cách xóa ký tự đầu tiên, trong khi "20202" có thể trở thành "2020" bằng cách xóa ký tự cuối cùng. Một cách tiếp cận ngây thơ có thể bỏ lỡ các vị trí này nếu nó chỉ kiểm tra "2020" dưới dạng chuỗi con liền kề. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xóa tất cả các chuỗi con có thể có, xây dựng lại chuỗi còn lại và so sánh nó với "2020". Đối với một chuỗi có độ dài`n`, có`n*(n+1)/2`chuỗi con, tạo ra độ phức tạp của`O(n^3)`để kiểm tra từng lần xóa. Điều này là không cần thiết đối với chuỗi mục tiêu nhỏ "2020" và cấu trúc của vấn đề. 

Thông tin chi tiết quan trọng là "2020" có độ dài 4, do đó, tối đa 4 ký tự của chuỗi gốc có liên quan đến việc tạo thành chuỗi đó. Chúng ta chỉ cần kiểm tra xem lần đầu tiên`k`ký tự của chuỗi và cuối cùng`4-k`ký tự tạo thành "2020" đối với một số`k`từ 0 đến 4. Điều này bao gồm tất cả các trường hợp trong đó một thao tác xóa liền kề có thể giữ nguyên "2020". Việc quan sát làm giảm vấn đề từ việc xem xét tất cả các thao tác xóa sang kiểm tra chính xác năm trường hợp trên mỗi chuỗi: chỉ tiền tố, hậu tố và sự kết hợp của tiền tố và hậu tố có tổng cộng bốn ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Kiểm tra tiền tố-hậu tố | O(1) mỗi chuỗi | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc chuỗi`s`và chiều dài của nó`n`. 
3. Kiểm tra xem`s`bản thân nó là "2020"; nếu vậy, hãy in "CÓ". 
4. Ngược lại, lặp lại`k`từ 0 đến 4, biểu thị số ký tự được lấy từ đầu`s`. Lấy cái đầu tiên`k`nhân vật và cái cuối cùng`4-k`các ký tự, nối chúng và so sánh với "2020". 
5. Nếu có bất kỳ sự kết hợp nào phù hợp, hãy in "CÓ"; nếu không khớp thì in "KHÔNG". 

Lý do điều này có tác dụng là vì bất kỳ phép biến đổi xóa một lần hợp lệ nào của`s`vào "2020" phải giữ nguyên một số tiền tố và một số hậu tố của chuỗi gốc. Vì độ dài của "2020" được cố định ở mức 4 nên các ký tự không được chạm vào chỉ có thể được phân bổ giữa phần đầu và phần cuối. Điều này đảm bảo rằng việc kiểm tra tất cả`k`từ 0 đến 4 bao gồm tất cả các thao tác xóa hợp lệ có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        target = "2020"
        found = False
        for k in range(5):
            if s[:k] + s[n-(4-k):] == target:
                found = True
                break
        print("YES" if found else "NO")

if __name__ == "__main__":
    solve()
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Đối với mỗi chuỗi, nó kiểm tra chính xác năm tổ hợp tiền tố-hậu tố tiềm năng đối với "2020". Các hoạt động cắt lát là an toàn vì`n >= 4`được đảm bảo. Vòng lặp sẽ ngắt ngay lập tức khi tìm thấy kết quả trùng khớp để tránh việc kiểm tra không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`"20192020"`| k | s[:k] | s[n-(4-k):] | Kết hợp | Cuộc thi đấu? | 
| --- | --- | --- | --- | --- | 
| 0 | "" | "2020" | "2020" | Có | 

Trận đấu diễn ra ngay lập tức với`k=0`. Đầu ra là "CÓ". 

### Ví dụ 2 

Chuỗi đầu vào:`"22019020"`| k | s[:k] | s[n-(4-k):] | Kết hợp | Cuộc thi đấu? | 
| --- | --- | --- | --- | --- | 
| 0 | "" | "9020" | "9020" | |
