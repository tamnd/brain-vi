---
title: "CF 1669B - Bộ ba"
description: "Bài toán yêu cầu chúng ta tìm số bất kỳ trong mảng xuất hiện ít nhất ba lần. Chúng ta được cung cấp nhiều trường hợp thử nghiệm và với mỗi trường hợp thử nghiệm, một mảng các số nguyên được cung cấp. Các số nguyên được đảm bảo nằm trong khoảng từ 1 đến kích thước của mảng."
date: "2026-06-10T02:00:49+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1669
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 784 (Div. 4)"
rating: 800
weight: 1669
solve_time_s: 447
verified: true
draft: false
---

[CF 1669B - Bộ ba](https://codeforces.com/problemset/problem/1669/B) 

**Đánh giá:** 800 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 7 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tìm số bất kỳ trong mảng xuất hiện ít nhất ba lần. Chúng ta được cung cấp nhiều trường hợp thử nghiệm và với mỗi trường hợp thử nghiệm, một mảng các số nguyên được cung cấp. Các số nguyên được đảm bảo nằm trong khoảng từ 1 đến kích thước của mảng. Nhiệm vụ của chúng ta là xuất ra một số như vậy nếu nó tồn tại hoặc -1 nếu không có số nào đạt tần số bằng ba. 

Các ràng buộc chỉ ra rằng số lượng ca kiểm thử có thể lên tới 10.000 và tổng kích thước mảng trên tất cả các ca kiểm thử không vượt quá 200.000. Điều này có nghĩa là bất kỳ giải pháp nào lặp lại các mảng nhiều lần cho mỗi trường hợp thử nghiệm hoặc thực hiện các vòng lặp lồng nhau đều có thể trở nên quá chậm. Do đó, chúng ta cần một cách tiếp cận với mức đạt tuyến tính cho mỗi trường hợp thử nghiệm hoặc nhiều nhất là O(n log n) cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh có thể khiến các giải pháp đơn giản bị lỗi bao gồm các mảng có ít hơn ba phần tử, trong đó không thể có bất kỳ giá trị nào xuất hiện ba lần. Ví dụ, một mảng`[1, 2]`sẽ ngay lập tức trả về -1. Một trường hợp tinh vi khác là khi nhiều số xuất hiện ba lần; vấn đề cho phép bất kỳ số hợp lệ nào, vì vậy chúng tôi chỉ cần xác định một số. Mảng trong đó tất cả các phần tử đều giống hệt nhau, chẳng hạn như`[5, 5, 5, 5]`, phải trả về chính xác giá trị đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp qua mọi giá trị có thể từ 1 đến n và đếm số lần xuất hiện của nó trong mảng. Phương pháp này đúng vì nó kiểm tra rõ ràng tất cả các phần tử, nhưng trong trường hợp xấu nhất, nếu n là 200.000, việc đếm từng số riêng lẻ sẽ dẫn đến các phép toán O(n^2), quá chậm so với các ràng buộc. 

Cách tiếp cận tối ưu sử dụng bộ đếm tần số để đếm tất cả các số trong một lượt. Điều này thúc đẩy thực tế là chúng ta chỉ cần biết liệu số đếm có đạt đến ba hay không, chứ không phải số đếm chính xác vượt quá ba. Việc sử dụng từ điển hoặc mảng được phân bổ trước có kích thước n+1 (vì tất cả các giá trị nằm trong khoảng từ 1 đến n) cho phép chúng tôi theo dõi tần số trong thời gian O(n) cho mỗi trường hợp thử nghiệm. Ngay khi bất kỳ số nào đạt đến số ba, chúng tôi có thể in ngay lập tức và dừng xử lý tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Bộ đếm tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case t. 
2. Với mỗi test, đọc n và mảng n số nguyên. 
3. Khởi tạo bộ đếm tần số. Nếu sử dụng danh sách, hãy phân bổ n+1 số 0 để theo dõi số lượng từ 1 đến n. 
4. Lặp qua từng số trong mảng. Tăng số lượng của nó trong bộ đếm tần số. 
5. Ngay khi một số đạt đến số ba, hãy in số đó và dừng xử lý trường hợp kiểm thử hiện tại. 
6. Nếu đến cuối mảng mà không có số nào đạt tới 3, hãy in -1. 

Thuật toán hoạt động vì bộ đếm tần số đảm bảo chúng tôi theo dõi chính xác số lần mỗi số xuất hiện. Bằng cách kiểm tra ngay lập tức khi số đếm đạt tới ba, chúng tôi đảm bảo rằng mọi đầu ra hợp lệ đều được in mà không cần tính toán không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        freq = [0] * (n + 1)
        found = False
        for num in arr:
            freq[num] += 1
            if freq[num] == 3:
                print(num)
                found = True
                break
        if not found:
            print(-1)

if __name__ == "__main__":
    solve()
```Mã đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`và khởi tạo một danh sách`freq`để đếm số lần xuất hiện của mỗi số. Bằng cách ngắt ngay lập tức khi một số đạt đến một
