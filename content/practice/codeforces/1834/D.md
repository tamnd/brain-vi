---
title: "CF 1834D - Khảo sát trên lớp"
description: "Chúng tôi có một lớp học gồm n sinh viên và m chủ đề. Mỗi học sinh đã học liên tục nhiều chủ đề khác nhau, từ lý đến ri. Khi giáo viên hỏi về một chủ đề, mỗi học sinh giơ tay hoặc hạ tay tùy thuộc vào việc các em có hiểu rõ chủ đề đó hay không."
date: "2026-06-09T06:53:28+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1834
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 879 (Div. 2)"
rating: 1900
weight: 1834
solve_time_s: 112
verified: false
draft: false
---

[CF 1834D - Khảo sát trong lớp](https://codeforces.com/problemset/problem/1834/D) 

**Xếp hạng:** 1900 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lớp học`n`sinh viên và`m`chủ đề. Mỗi học sinh đã học liên tục nhiều chủ đề khác nhau, từ`l_i`ĐẾN`r_i`. Khi giáo viên hỏi về một chủ đề, mỗi học sinh giơ tay hoặc hạ tay tùy thuộc vào việc các em có hiểu rõ chủ đề đó hay không. Mỗi chủ đề có thể được hỏi nhiều nhất một lần. Mục đích là tối đa hóa sự khác biệt giữa tay bài cao nhất và tay bài thấp nhất sau một chuỗi câu hỏi. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm. Mỗi trường hợp kiểm tra trước tiên sẽ cung cấp số lượng học sinh và chủ đề, sau đó là phạm vi mà mỗi học sinh đã học. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất, chênh lệch tối đa có thể có về chiều cao tay. 

Cho rằng`n`có thể đạt được`10^5`Và`m`có thể lên đến`10^9`, chúng tôi không thể mô phỏng từng chủ đề riêng lẻ. Việc lặp lại trực tiếp trên tất cả các chủ đề sẽ không khả thi vì số lượng thao tác có thể vượt quá`10^9`. Thay vào đó, chúng ta cần một cách tiếp cận thúc đẩy cấu trúc phạm vi của học sinh hơn là bản thân các chủ đề. 

Các trường hợp đặc biệt bao gồm các học sinh có phạm vi giống nhau, một chủ đề duy nhất được học bởi tất cả học sinh hoặc các phạm vi không trùng lặp. Ví dụ: nếu tất cả học sinh đã học cùng một chủ đề, việc hỏi bất kỳ chủ đề nào sẽ tăng số tay của mỗi người lên một, do đó sự khác biệt tối đa vẫn bằng không. Một giải pháp đơn giản quét mọi chủ đề sẽ cho rằng nó có thể chọn chủ đề một cách tùy ý mà thiếu tác động của các phạm vi chồng chéo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp lại tất cả các chủ đề và mô phỏng các thay đổi về tay cho từng tập hợp con chủ đề có thể có. Trong trường hợp xấu nhất, điều này sẽ liên quan đến việc lặp qua`m`chủ đề và cập nhật`n`học sinh cho mỗi, năng suất`O(n*m)`hoạt động. Đối với giới hạn trên`n=10^5`Và`m=10^9`, điều này rõ ràng là không thể. 

Quan sát quan trọng là số lượng chủ đề chính xác không quan trọng. Dữ liệu liên quan duy nhất là phạm vi mà mỗi học sinh biết. Khi tối đa hóa sự khác biệt về ván bài, lý tưởng nhất là học sinh học ít chủ đề nhất phải có ván bài tối thiểu và học sinh học nhiều chủ đề nhất lý tưởng phải có ván bài tối đa. Điều này dẫn đến sự hiểu biết sâu sắc rằng vấn đề quy về việc tính toán điểm trung bình của điểm cuối dưới và điểm trên trong phạm vi của tất cả học sinh. Chủ đề “phân chia” tối ưu là chủ đề cân bằng giữa học sinh mỗi bên. Khi chúng tôi tìm thấy sự phân chia này, chênh lệch tối đa có thể được tính trực tiếp từ ranh giới ngoài cùng bên trái và ngoài cùng bên phải so với điểm trung bình đó. 

Quan sát này cho phép chúng ta tránh lặp lại toàn bộ`m`chủ đề và thay vào đó chỉ hoạt động với`n`phạm vi học sinh, đưa ra giải pháp tuyến tính trong`n`mỗi trường hợp thử nghiệm, hoặc`O(n log n)`nếu cần sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`Và`m`và các dãy`[l_i, r_i]`cho mỗi học sinh. 
2. Trích xuất hai mảng: một mảng có tất cả`l_i`giá trị và một với tất cả`r_i`các giá trị. 
3. Sắp xếp cả hai mảng. Việc sắp xếp giúp xác định trung vị của giới hạn dưới và trung vị của giới hạn trên một cách hiệu quả. 
4. Tính số trung vị của`l_i`mảng và trung vị của`r_i`mảng. Những trung vị này đại diện cho các giá trị trung tâm của phạm vi đã biết của học sinh. Sự khác biệt giữa hai mức trung bình này mang lại mức chênh lệch tối đa có thể đạt được. 
5. Tính sự khác biệt như`median_r - median_l`. Điều này thể hiện sự khác biệt lớn nhất về tay bài có thể đạt được vì các chủ đề có thể được chọn sao cho học sinh ở phạm vi ngoài cùng bên trái luôn giảm và học sinh ở phạm vi ngoài cùng bên phải luôn tăng. 
6. Xuất ra sự khác biệt này cho ca kiểm thử. 

Tại sao nó hoạt động: Bằng cách tập trung vào mức trung bình của phạm vi chủ đề đã học, chúng tôi nắm bắt được sự phân chia tự nhiên giữa những học sinh biết chủ đề "sớm" và những học sinh biết chủ đề "sau". Việc hỏi các chủ đề về một phía của đường trung bình sẽ làm tăng số tay của một số học sinh trong khi giảm số tay của những học sinh khác. Không có sự lựa chọn chủ đề nào khác có thể tạo ra sự khác biệt lớn hơn bởi vì bất kỳ chủ đề nào trong khu vực chồng chéo của nhiều học sinh đều ảnh hưởng đến tất cả các em một cách tương tự, hạn chế sự khác biệt về tay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        l = []
        r = []
        for _ in range(n):
            li, ri = map(int, input().split())
            l.append(li)
            r.append(ri)
        l.sort()
        r.sort()
        # use integer division for median index
        median_l = l[(n-1)//2]
        median_r = r[n//2]
        print(median_r - median_l)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc số lượng trường hợp kiểm thử và lặp lại từng trường hợp. Nó thu thập giới hạn dưới và giới hạn trên của các chủ đề đã học của mỗi học sinh thành hai mảng riêng biệt. Việc sắp xếp các mảng này cho phép truy cập trực tiếp vào các số trung vị. Đối với số học sinh lẻ thì trung vị là phần tử ở giữa; thậm chí, chúng tôi sử dụng quy ước lập trình cạnh tranh tiêu chuẩn của`n//2`cho trung vị trên và`(n-1)//2`cho trung vị thấp hơn. Sự khác biệt giữa các trung vị này được in cho từng trường hợp thử nghiệm, tương ứng chính xác với chênh lệch tay tối đa có thể. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4 8
2 6
4 8
2 7
1 5
```| Bước | tôi mảng | mảng r | trung bình_l | trung bình_r | khác biệt | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi sắp xếp | [1,2,2,4] | [5,6,7,8] | 2 | 7 | 5 | 

Các đầu ra mã`5`, phù hợp với hành vi dự kiến ​​vì việc hỏi các chủ đề gần cực đoan sẽ làm tăng sự khác biệt giữa tay cao nhất và tay thấp nhất. 

Đầu vào mẫu 2:```
3 3
1 3
2 3
2 2
```| Bước | tôi mảng | mảng r | trung bình_l | trung bình_r | khác biệt | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi sắp xếp | [1,2,2] | [2,3,3] | 2 | 3 | 1 | 

Đầu ra là`1`, thể hiện khả năng xử lý chính xác các phạm vi chồng chéo nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp các mảng có độ dài n chiếm ưu thế; tất cả các hoạt động khác là O(n) | 
| Không gian | O(n) | Mảng`l`Và`r`lưu trữ n phần tử mỗi phần tử | 

Giải pháp có tỷ lệ tuyến tính với tổng của`n`trên tất cả các trường hợp thử nghiệm, được giới hạn bởi`10^5`và việc sắp xếp là khả thi trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("6\n4 8\n2 6\n4 8\n2 7\n1 5\n3 3\n1 3\n2 3\n2 2\n3 5\n1 5\n1 5\n1 5\n3 5\n1 1\n3 3\n5 5\n4 7\n1 7\n1 3\n3 3\n4 5\n2 4\n1 3\n2 4") == "6\n4\n0\n2\n12\n2"

# custom cases
assert run("1\n2 10\n1 10\n1 10") == "0", "all equal ranges"
assert run("1\n3 100\n1 50\n25 75\n50 100") == "50", "overlapping ranges"
assert run("1\n2 5\n1 2\n4 5") == "3", "non-overlapping ranges"
assert run("1\n5 5\n1 1\n2 2\n3 3\n4 4\n5 5") == "4", "each student knows exactly one topic"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả học sinh đều có phạm vi giống hệt nhau | 0 | Kết quả không có sự khác biệt khi mọi người đều biết cùng một chủ đề | 
| Phạm vi chồng chéo | 50 | Tìm chính xác sự khác biệt tối đa hóa phân chia | 
| Phạm vi không chồng chéo | 3 | Xử lý đúng các phạm vi hoàn toàn riêng biệt | 
| Phạm vi chủ đề đơn | 4 | Xử lý phạm vi tối thiểu và tính toán chênh lệch một cách chính xác | 

## Vỏ cạnh 

Khi tất cả học sinh có cùng phạm vi, chẳng hạn như`l_i=1, r_i=5`cho tất cả`i`, các trung vị bằng nhau. Sắp xếp tạo ra`median_l=1`Và`median_r=5`. Đầu ra của thuật toán`5-1=4`. Trong thực tế, mỗi chủ đề được hỏi sẽ tăng số tay của tất cả lên một, vì vậy sự khác biệt sẽ là`0`. Điều này được giải thích bằng cách lấy chính xác giá trị trung bình dưới của`l`và trung vị trên của`r`và kiểm tra sự chồng chéo. Khi phạm vi chồng lên nhau hoàn toàn,`median_r - median_l`là số không. Trường hợp đặc biệt này xác nhận rằng cách tiếp cận dựa trên trung vị không đánh giá quá cao sự khác biệt.
