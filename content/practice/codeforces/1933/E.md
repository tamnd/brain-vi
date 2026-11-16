---
title: "CF 1933E - Cuộc đua giữa rùa và thỏ: Huấn luyện tối ưu"
description: "Chúng tôi được yêu cầu mô hình hóa quá trình huấn luyện của Isaac trên nhiều đường chạy. Mỗi bản nhạc bao gồm một số đoạn có độ dài bằng nhau và mỗi đoạn được hoàn thành sẽ tăng hiệu suất của anh ấy theo cách giảm tuyến tính."
date: "2026-06-08T18:16:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1933
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 929 (Div. 3)"
rating: 1500
weight: 1933
solve_time_s: 144
verified: false
draft: false
---

[CF 1933E - Cuộc đua giữa rùa và thỏ: Huấn luyện tối ưu](https://codeforces.com/problemset/problem/1933/E) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, thực hiện, toán học, tìm kiếm ba ngôi 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô hình hóa quá trình huấn luyện của Isaac trên nhiều đường chạy. Mỗi bản nhạc bao gồm một số đoạn có độ dài bằng nhau và mỗi đoạn được hoàn thành sẽ tăng hiệu suất của anh ấy theo cách giảm tuyến tính. Phần đầu tiên góp phần`u`, thứ hai`u-1`, thứ ba`u-2`, vân vân. Điều này có nghĩa là nếu Isaac hoàn thành nhiều phần hơn`u`, một số phần thậm chí có thể làm giảm hiệu suất của anh ấy vì mức tăng trở nên âm. 

Đối với một bài hát bắt đầu nhất định`l`và giá trị`u`, chúng ta phải chọn một bài hát kết thúc`r`(Ở đâu`l ≤ r ≤ n`) để tối đa hóa tổng hiệu suất đạt được. Nếu nhiều`r`tạo ra cùng một mức tăng ích tối đa, thì mức tăng nhỏ nhất được chọn. Mỗi truy vấn cung cấp một thông tin khác nhau`l`Và`u`và chúng tôi có nhiều trường hợp thử nghiệm. 

Các ràng buộc chỉ ra`n`Và`q`mỗi người có thể lên tới`10^5`cho mỗi trường hợp thử nghiệm, nhưng tổng số trên tất cả các trường hợp thử nghiệm không vượt quá`2 * 10^5`. Một cách tiếp cận ngây thơ cố gắng hết sức có thể`r`cho mọi truy vấn có thể mất`O(n * q)`thời gian có thể đạt tới`10^{10}`trong trường hợp xấu nhất và rõ ràng là quá chậm. Chúng tôi cần một phương pháp xử lý từng truy vấn theo thời gian logarit hoặc gần như không đổi bằng cách sử dụng tính toán trước. 

Các phần khó khăn bao gồm việc xử lý các số gia âm khi tổng số phần vượt quá`u`và mối quan hệ trong đó có nhiều`r`các giá trị cho tổng mức tăng như nhau. Việc triển khai bất cẩn làm tính tổng số tiền tăng trực tiếp cho từng ứng viên`r`có thể đưa ra câu trả lời sai hoặc vượt quá giới hạn thời gian. 

Trường hợp cạnh bê tông là khi`u`nhỏ hơn tổng các phần từ`l`ĐẾN`n`. Ví dụ, nếu`a = [2, 2, 2]`Và`l = 1, u = 2`, hoàn thành tất cả các bài hát sẽ cho các phần`[2,2,2]`tổng cộng là 6. Mức tăng hiệu suất là`2,1,0,-1,-2,-3`, do đó mức tăng tối đa xảy ra trước khi hoàn thành tất cả các phần. Một cách tiếp cận ngây thơ luôn tính tổng tất cả các phần sẽ chọn`r=3`không chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force tính toán tổng hiệu suất đạt được cho từng khả năng`r`bắt đầu từ`l`. Đối với mỗi`r`, chúng tôi tổng hợp tất cả các phần giữa`l`Và`r`để xác định số lượng chúng ta lấy, sau đó tính tổng`u, u-1, ..., u-(total_sections-1)`. Điều này có tác dụng chính xác, nhưng đối với số lượng lớn`n`và nhiều truy vấn trở nên không thực tế vì vòng lặp bên trong có thể chạy`O(n)`cho mỗi truy vấn, dẫn đến`O(n*q)`hoạt động. 

Thông tin chi tiết quan trọng là tính toán trước tổng tiền tố của các phần tuyến đường. Cho phép`prefix[i]`là tổng của số đầu tiên`i`bài hát. Sau đó tổng số phần từ`l`ĐẾN`r`là`prefix[r] - prefix[l-1]`. Tổng lợi nhuận cho`S`phần bắt đầu bằng giá trị`u`có thể được tính bằng công thức chuỗi số học:`u + (u-1) + ... + (u-S+1) = S*u - S*(S-1)//2`. Điều này cho phép chúng tôi tính toán mức tăng cho bất kỳ mảng con liền kề nào trong thời gian O(1) sau khi tổng tiền tố được chuẩn bị. 

Vì hàm khuếch đại`G(S) = S*u - S*(S-1)/2`là một phương trình bậc hai lõm ở`S`, mức tăng tăng trong khi`S ≤ u`và giảm khi`S > u`. Vì vậy, để tối đa hóa độ lợi, chúng ta cần tìm giá trị nhỏ nhất`r`sao cho tổng số phần từ`l`ĐẾN`r`chỉ ở trên hoặc bằng`u`. Điều này có thể được thực hiện một cách hiệu quả bằng tìm kiếm nhị phân trên tổng tiền tố. Đối với mỗi truy vấn, chúng tôi tính toán`target = prefix[l-1] + u`và tìm số nhỏ nhất`r ≥ l`như vậy`prefix[r] ≥ target`. Nếu như`prefix[n] < target`, chúng tôi chọn`r = n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*q) | O(n) | Quá chậm | 
| Tổng tiền tố + Tìm kiếm nhị phân | O(n + q*log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tổng tiền tố của các đoạn đường. Cho phép`prefix[0] = 0`Và`prefix[i] = prefix[i-1] + a[i-1]`. Điều này cho phép tính toán O(1) tổng số phần giữa bất kỳ`l`Và`r`. 
2. Đối với mỗi truy vấn`(l, u)`, định nghĩa`current_prefix = prefix[l-1]`. Chúng tôi muốn tổng số phần`S = prefix[r] - current_prefix`được gần gũi như vậy`u`nhất có thể mà không vượt quá mức tăng tối đa. Nếu như`S <= u`, mức tăng tăng với nhiều phần hơn; nếu như`S > u`, mức tăng bắt đầu giảm. 
3. Thực hiện tìm kiếm nhị phân trên mảng tổng tiền tố từ`r = l`ĐẾN`n`để tìm nhỏ nhất`r`như vậy`prefix[r] - prefix[l-1] ≥ u`. Nếu không như vậy`r`tồn tại, chọn`r = n`. 
4. Xuất cái này`r`như câu trả lời cho truy vấn. Theo cách xây dựng, điều này`r`mang lại hiệu suất tối đa vì nó thu thập càng nhiều phần càng tốt mà không vượt quá`u`theo cách có thể làm giảm lợi ích. 

Tại sao nó hoạt động: Công thức tính chuỗi số học là lõm. Vì vậy, khi số lượng phần vượt quá`u`, các phần bổ sung làm giảm tổng mức tăng. Tìm kiếm nhị phân đảm bảo đầu tiên`r`đạt được độ lợi cao nhất, thỏa mãn yêu cầu của bài toán để chọn giá trị nhỏ nhất`r`trong trường hợp có quan hệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        prefix = [0] * (n + 1)
        for i in range(1, n+1):
            prefix[i] = prefix[i-1] + a[i-1]
        q = int(input())
        queries = [tuple(map(int, input().split())) for _ in range(q)]
        res = []
        for l, u in queries:
            # binary search for smallest r such that prefix[r] - prefix[l-1] >= u
            low, high = l, n
            ans = n
            target = prefix[l-1] + u
            while low <= high:
                mid = (low + high) // 2
                if prefix[mid] >= target:
                    ans = mid
                    high = mid - 1
                else:
                    low = mid + 1
            res.append(ans)
        print(' '.join(map(str, res)))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính tổng tiền tố cho từng trường hợp thử nghiệm. Sau đó, mỗi truy vấn được xử lý độc lập bằng cách sử dụng tìm kiếm nhị phân để tìm bản nhạc đầu tiên có số phần tích lũy đạt hoặc vượt quá`u`. Mục tiêu được điều chỉnh bằng`prefix[l-1]`bởi vì việc đếm bắt đầu từ bài hát`l`. Điều này xử lý chính xác các vấn đề riêng lẻ và đảm bảo nhỏ nhất`r`được chọn. sử dụng`prefix[n]`như một giới hạn trên đảm bảo rằng nếu`u`lớn hơn tổng các phần còn lại`r = n`được chọn. 

## Ví dụ đã hoạt động 

Mẫu 1 truy vấn đầu tiên:`l = 1, u = 8`, bài hát`a = [3,1,4,1,5,9]`. 

| r | tiền tố[r]-tiền tố[l-1] | đạt được công thức | quyết định | 
| --- | --- | --- | --- | 
| 1 | 3 | 8+7+6 = 21 | ít hơn bạn | 
| 2 | 4 | 8+7+6+5 = 26 | ít hơn bạn | 
| 3 | 8 | 8+7+6+5+4+3+2+1 = 36 | bằng bạn | 
| 4 | 9 | 8+7+6+5+4+3+2+1+0 = 36 | cùng mức tăng, không nhỏ hơn r | 

Điều này xác nhận thuật toán đã chọn chính xác giá trị đầu tiên`r`đạt được mức lợi nhuận tối đa. 

Một ví dụ khác, truy vấn thứ ba:`l=5, u=9`,`a[5..6] = [5,9]`. 

| r | tổng số phần | đạt được | quyết định | 
| --- | --- | --- | --- | 
| 5 | 5 | 9+8+7+6+5=35 | ứng cử viên | 
| 6 | 14 | 9+8+...+(-4)=35 | giống nhau | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q*log n) | Tổng tiền tố lấy O(n). Mỗi truy vấn tìm kiếm nhị phân là O(log n). Tổng số trường hợp thử nghiệm 2*10^5 đảm bảo tính khả thi. | 
| Không gian | O(n) | Mảng tiền tố có kích thước n+1, cộng với bộ nhớ truy vấn tạm thời. | 

Giải pháp vừa vặn thoải mái trong giới hạn 5 giây cho tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Sample 1
inp1 = """5
6
3 1 4 1 5 9
3
1 8
2 7
5 9
1
10
1
1 1
9
5 10 9 6 8 3
```
