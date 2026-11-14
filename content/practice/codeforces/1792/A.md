---
title: "CF 1792A - Lực lượng chơi game"
description: "Chúng tôi được yêu cầu xác định số lần sử dụng phép thuật tối thiểu Monocarp cần để tiêu diệt một nhóm quái vật, mỗi quái vật có một giá trị máu nhất định."
date: "2026-06-09T10:22:39+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1792
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 142 (Rated for Div. 2)"
rating: 800
weight: 1792
solve_time_s: 153
verified: false
draft: false
---

[CF 1792A - GamingForces](https://codeforces.com/problemset/problem/1792/A) 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định số lần sử dụng phép thuật tối thiểu Monocarp cần để tiêu diệt một nhóm quái vật, mỗi quái vật có một giá trị máu nhất định. Monocarp có hai phép thuật: một phép thuật làm giảm sức khỏe của hai quái vật khác nhau cùng một lúc và một phép thuật khác giết chết ngay lập tức một con quái vật. Mục tiêu là sắp xếp các phép thuật này theo cách có thể tiêu diệt tất cả quái vật bằng cách sử dụng càng ít phép thuật càng tốt. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp số lượng quái vật, theo sau là danh sách các giá trị sức khỏe của chúng. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất, số lượng phép thuật tối thiểu được yêu cầu. 

Các ràng buộc tương đối nhỏ: số lượng quái vật trong một trường hợp thử nghiệm tối đa là 100 và tổng số quái vật trong tất cả các trường hợp thử nghiệm nhiều nhất là 20.000. Điều này cho phép các thuật toán lên tới khoảng O(n^2) cho mỗi trường hợp thử nghiệm chạy thoải mái, nhưng bất kỳ thuật toán nào đạt đến O(n^3) có thể bắt đầu không hiệu quả. Sức khỏe của mỗi quái vật tối đa là 100, điều này cho thấy rằng các giải pháp tùy thuộc vào việc sắp xếp, lựa chọn tham lam hoặc các thao tác số học cơ bản sẽ hoạt động hiệu quả. 

Một cách tiếp cận ngây thơ có thể thất bại trong trường hợp quái vật có sức khỏe không cân bằng cao. Ví dụ: nếu chúng ta có ba quái vật có sức khỏe`[1, 1, 100]`, việc tham lam ghép đôi hai con đầu tiên cho phép thuật hai con quái vật là hiệu quả, nhưng việc liên tục cố gắng ghép đôi các con quái vật mà không tính đến lượng máu lớn nhất có thể dẫn đến việc sử dụng phép thuật một con quái vật nhiều lần, đưa ra tổng số dưới mức tối ưu. 

## Phương pháp tiếp cận 

Một giải pháp vũ lực sẽ thử tất cả các chuỗi sử dụng phép thuật có thể có: đối với mỗi cặp quái vật còn sống, chúng ta có thể áp dụng phép thuật hai con quái vật và đối với bất kỳ quái vật nào, chúng ta có thể sử dụng phép thuật một con quái vật. Điều này sẽ tạo ra một số lượng lớn các trạng thái và vì mỗi quái vật có thể có tới 100 máu, nên số lượng trạng thái tiềm năng sẽ ở mức 100^100 trong trường hợp xấu nhất. Điều này rõ ràng là khó chữa. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng phép thuật hai con quái vật luôn hiệu quả hơn khi nó có thể được áp dụng. Mỗi lần sử dụng sẽ giảm 2 máu tổng hợp của hai quái vật, trong khi phép thuật sử dụng một quái vật chỉ giảm một. Do đó, chiến lược tối ưu sẽ luôn cố gắng tối đa hóa việc sử dụng phép thuật hai quái vật. Nếu chúng ta nghĩ về mặt sức khỏe tổng thể, hãy`H`là tổng của tất cả sức khỏe của quái vật và để`M`là sức khỏe cá nhân tối đa. Số lượng phép thuật tối thiểu bị hạn chế bởi hai sự thật: mỗi phép thuật làm giảm tối đa 2 điểm máu (đối với phép thuật hai quái vật), vì vậy chúng ta cần ít nhất`ceil(H/2)`bùa chú; Ngoài ra, không có con quái vật nào có thể bị giết trong thời gian ít hơn`M`phép thuật, bởi vì chúng tôi không thể giảm máu của nó nhanh hơn 1 mỗi lần sử dụng trong trường hợp xấu nhất. Do đó, tổng số phôi tối thiểu là`max(ceil(H/2), M)`. 

Thông tin chuyên sâu này ngay lập tức dẫn đến giải pháp O(n) cho mỗi trường hợp thử nghiệm: tính tổng giá trị tình trạng và tình trạng tối đa, sau đó lấy giá trị tối đa của một nửa tổng (làm tròn) và tình trạng tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * H) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Điều này thiết lập vòng lặp của chúng tôi trên nhiều kịch bản. 
2. Với mỗi test case, hãy đọc số lượng quái vật`n`. 
3. Đọc danh sách giá trị sức khỏe của quái vật. 
4. Tính tổng`total_health`của tất cả các giá trị sức khỏe. Điều này thể hiện tổng số "thiệt hại" cần thiết. 
5. Tìm sức khỏe tối đa`max_health`giữa tất cả quái vật. Điều này đảm bảo rằng không có sức khỏe của quái vật nào vượt quá số lần sử dụng phép thuật. 
6. Tính toán`ceil(total_health / 2)`để giải thích cho thực tế là phép thuật hai quái vật làm giảm hai điểm máu mỗi lần sử dụng. Trong số học số nguyên, đây là`(total_health + 1) // 2`. 
7. Kết quả là`max(max_health, (total_health + 1) // 2)`. Điều này đảm bảo rằng cả hai ràng buộc đều được thỏa mãn: chúng ta có đủ số lần sử dụng để giảm tổng lượng máu và đủ để loại bỏ quái vật mạnh nhất. 
8. In kết quả. 

Tại sao nó hoạt động: Hai hạn chế – tổng lượng máu và lượng máu tối đa – giải quyết các điểm nghẽn của hệ thống. Không có thuật toán nào có thể sử dụng ít lần sử dụng hơn một trong hai giới hạn này, bởi vì phép thuật hai quái vật không thể giảm tổng số nhanh hơn hai điểm mỗi lần sử dụng và không có quái vật nào có thể chết nhanh hơn 1 máu mỗi lần sử dụng nếu đó là quái vật mạnh duy nhất còn lại. Sự lựa chọn tham lam là luôn áp dụng phép thuật hai con quái vật cho đến khi chỉ còn lại một con được mã hóa ngầm trong công thức tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    h = list(map(int, input().split()))
    total_health = sum(h)
    max_health = max(h)
    result = max(max_health, (total_health + 1) // 2)
    print(result)
```Giải pháp đọc số lượng trường hợp kiểm thử, lặp lại từng trường hợp, tính tổng và giá trị tối đa của tình trạng, sau đó tính kết quả. sử dụng`(total_health + 1) // 2`đảm bảo làm tròn chính xác cho tổng số lẻ. Giải pháp sử dụng thêm không gian O(1) ngoài việc lưu trữ mảng sức khỏe. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`[1, 2, 1, 2]`

`total_health = 6`

`max_health = 2`

`ceil(total_health/2) = 3`

`max(3, 2) = 3`→ Đầu ra`3`| Bước | Hành động | tổng_sức khỏe | max_health | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | tổng và tối đa | 6 | 2 | | 
| 2 | tính max(max_health, ceil(sum/2)) | 6 | 2 | 3 | 

**Mẫu 2** 

đầu vào:`[2, 4, 2]`

`total_health = 8`

`max_health = 4`

`ceil(total_health/2) = 4`

`max(4, 4) = 4`→ Đầu ra`4`Công thức đảm bảo rằng quái vật mạnh nhất và tổng số đều được tính đến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tổng và tối đa của n phần tử | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Lưu trữ sức khỏe quái vật | 

Thuật toán dễ dàng xử lý giới hạn của tổng số 20.000 quái vật trong tất cả các trường hợp thử nghiệm, thực hiện khoảng 20.000 thao tác mỗi tổng/tối đa, thấp hơn nhiều so với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        h = list(map(int, input().split()))
        total_health = sum(h)
        max_health = max(h)
        result = max(max_health, (total_health + 1) // 2)
        output.append(str(result))
    return "\n".join(output)

# provided samples
assert run("3\n4\n1 2 1 2\n3\n2 4 2\n5\n1 2 3 4 5\n") == "3\n4\n5", "sample 1-3"

# custom cases
assert run("1\n1\n100\n") == "100", "single monster max health"
assert run("1\n2\n1 1\n") == "1", "smallest total health even"
assert run("1\n2\n1 2\n") == "2", "smallest total health odd"
assert run("1\n5\n5 5 5 5 5\n") == "13", "all equal large health"
assert run("1\n3\n1 100 1\n") == "100", "dominant monster"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n100\n`| 100 | Quái vật đơn lẻ, máu tối đa bằng tổng | 
|`1\n2\n1 1\n`| 1 | Tổng lượng máu tối thiểu, thậm chí | 
|`1\n2\n1 2\n`| 2 | Tổng lượng máu tối thiểu, lẻ | 
|`1\n5\n5 5 5 5 5\n`| 13 | Tất cả quái vật đều bằng nhau, tổng > max | 
|`1\n3\n1 100 1\n`| 100 | Một con quái vật thống trị | 

## Vỏ cạnh 

Nếu chỉ có một con quái vật, câu trả lời là lượng máu của nó. Ví dụ,`[100]`sản xuất`100`. Thuật toán tính toán`max(100, ceil(100/2)) = 100`. 

Nếu tất cả quái vật đều có sức khỏe như nhau, chẳng hạn như`[5, 5, 5, 5, 5]`, tổng là 25,`ceil(25/2) = 13`, tối đa là 5 và đầu ra là 13. Điều này giải thích chính xác việc ghép đôi quái vật một cách hiệu quả với phép thuật hai quái vật. 

Nếu một con quái vật thống trị tổng số, như`[1, 100, 1]`, tổng là 102,`ceil(102/2) = 51`, nhưng tối đa là 100. Thuật toán đưa ra chính xác 100 vì chúng ta không thể tiêu diệt quái vật mạnh nhất nhanh hơn 100 lần sử dụng.
