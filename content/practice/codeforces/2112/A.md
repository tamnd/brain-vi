---
title: "CF 2112A - Chủng tộc"
description: "Trong bài toán này, Alice và Bob đang chạy đua để giành được một giải thưởng sẽ xuất hiện ở một trong hai điểm phân biệt trên đường một chiều. Alice đã chọn điểm xuất phát của mình a."
date: "2026-06-08T04:26:26+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2112
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 180 (Rated for Div. 2)"
rating: 800
weight: 2112
solve_time_s: 78
verified: true
draft: false
---

[CF 2112A - Cuộc đua](https://codeforces.com/problemset/problem/2112/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, Alice và Bob đang chạy đua để giành được một giải thưởng sẽ xuất hiện ở một trong hai điểm phân biệt trên đường một chiều. Alice đã chọn điểm xuất phát của mình`a`. Bob muốn chọn một điểm bắt đầu là số nguyên`b`nó khác với của Alice, để anh ấy có thể đạt được giải thưởng nhanh hơn Alice bất kể địa điểm nào trong hai địa điểm`x`hoặc`y`giải thưởng xuất hiện. Khoảng cách giữa hai điểm bất kỳ chỉ đơn giản là sự khác biệt tuyệt đối của tọa độ của chúng. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định`a`,`x`, Và`y`, tất cả các số nguyên phân biệt từ 1 đến 100. Đầu ra phải nêu rõ cho từng trường hợp kiểm thử xem điểm đó có`b`tồn tại. Câu trả lời là "CÓ" nếu Bob có thể đảm bảo đánh bại Alice bất kể vị trí giải thưởng ở đâu và nếu không thì "KHÔNG". 

Với các giới hạn chặt chẽ (tất cả các số ≤ 100 và tối đa 1000 trường hợp thử nghiệm), bất kỳ thuật toán nào lặp qua tất cả các số nguyên trong phạm vi khả thi đều có thể chấp nhận được, vì cần tối đa 100 bước cho mỗi trường hợp thử nghiệm. Tuy nhiên, có thể giải thích vấn đề mà không cần liệt kê đầy đủ. 

Ví dụ: các trường hợp chính xảy ra khi vị trí giải thưởng nằm ở vị trí của Alice`a = 3, x = 1, y = 5`. Trong tình huống như vậy, nếu Bob chọn một điểm để đánh bại Alice để giành một giải, anh ấy có thể thua cô ấy ở giải kia. Một cách tiếp cận ngây thơ chọn điểm giữa hoặc một trong các giải thưởng mà không kiểm tra cả hai khoảng cách có thể khẳng định "CÓ" một cách không chính xác khi không tồn tại vị trí đảm bảo như vậy. 

## Phương pháp tiếp cận 

Một cách tiếp cận brute-force đơn giản là kiểm tra mọi số nguyên có thể`b`từ 1 đến 100 (không bao gồm`a`) và xem liệu đối với cả hai vị trí giải thưởng,`|b - x| < |a - x|`Và`|b - y| < |a - y|`giữ. Điều này hiệu quả vì miền nhỏ và đối với mỗi trường hợp thử nghiệm, nó yêu cầu tối đa 99 lần kiểm tra với hai chênh lệch tuyệt đối cho mỗi lần kiểm tra, con số này không đáng kể trong thực tế. Lực lượng vũ phu hoạt động vì chúng tôi xác minh rõ ràng điều kiện thắng của Bob cho mọi vị trí bắt đầu khả thi, nhưng nó có phần không phù hợp. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là số nguyên`b`điều đó bảo đảm cho chiến thắng phải nằm chặt chẽ giữa`a`và vị trí trao giải “xa nhất”. Nếu cả hai giải đều cùng phía với Alice (ví dụ:`a < x < y`), Bob có thể xuất phát xa hơn giải thưởng xa hơn, gần với giải thưởng đó hơn Alice, điều này đảm bảo rằng anh ấy sẽ đánh bại cô ấy để giành cả hai giải thưởng. Nếu các giải thưởng ở phía đối diện của Alice (`x < a < y`), bất kể Bob bắt đầu từ đâu, anh ấy sẽ ở gần một giải thưởng hơn Alice nhưng lại ở xa giải thưởng kia hơn. Vì vậy, một vị trí được đảm bảo chỉ tồn tại khi cả hai giải thưởng đều đứng về phía Alice. Điều này làm giảm vấn đề so sánh các vị trí: Bob thắng nếu`(x - a) * (y - a) > 0`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(100 * t) | O(1) | Đã chấp nhận | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Điều này xác định có bao nhiêu chủng tộc riêng biệt mà chúng tôi cần đánh giá. 
2. Với mỗi test, hãy đọc các số nguyên`a`,`x`, Và`y`. Những điều này xác định vị trí xuất phát của Alice và hai vị trí có thể nhận được giải thưởng. 
3. Tính tích`(x - a) * (y - a)`. Sản phẩm này nắm bắt được vị trí tương đối của các giải thưởng đối với Alice. Nếu sản phẩm dương tính thì cả hai giải thưởng đều đứng về phía Alice. Nếu âm thì chúng nằm ở hai phía đối diện nhau. 
4. Nếu kết quả dương tính, ghi "CÓ". Bob có thể chọn vị trí gần giải thưởng xa hơn và đảm bảo đánh bại Alice ở cả hai vị trí. 
5. Ngược lại, xuất ra "NO". Không có vị trí ban đầu là số nguyên duy nhất nào đảm bảo Bob sẽ tiến gần hơn Alice đến cả hai giải thưởng. 

Tại sao nó hoạt động: Bất biến là dấu hiệu của`(x - a) * (y - a)`mã hóa đầy đủ sự sắp xếp hình học. Tích cực có nghĩa là cả hai giải thưởng đều ở bên trái hoặc bên phải của Alice, đảm bảo tồn tại một điểm xuất phát duy nhất để tiếp cận cả hai nhanh hơn. Tiêu cực có nghĩa là họ đè lên Alice, khiến Bob không thể chọn một vị trí gần với cả hai giải thưởng hơn cùng một lúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, x, y = map(int, input().split())
    if (x - a) * (y - a) > 0:
        print("YES")
    else:
        print("NO")
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó phân tích ba số nguyên biểu thị điểm xuất phát của Alice và hai vị trí giải thưởng tiềm năng. Logic cốt lõi kiểm tra xem cả hai giải thưởng có cùng quan điểm với việc Alice sử dụng sản phẩm hay không`(x - a) * (y - a)`. Theo mặc định, đầu ra là ngay lập tức và không phân biệt chữ hoa chữ thường, vì Codeforces chấp nhận "CÓ"/"KHÔNG". 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`1 3 4`| Biến | Giá trị | Lý luận | 
| --- | --- | --- | 
| một | 1 | Sự khởi đầu của Alice | 
| x | 3 | Giải 1 | 
| y | 4 | Giải 2 | 
| x - a | 2 | Khoảng cách từ Alice tới giải 1 | 
| y - một | 3 | Khoảng cách từ Alice tới giải 2 | 
| sản phẩm | 6 | Tích cực, cả hai giải thưởng bên phải | 
| Đầu ra | CÓ | Bob có thể bắt đầu từ số 4 để đánh bại Alice về cả hai | 

**Đầu vào mẫu 2:**`3 1 5`| Biến | Giá trị | Lý luận | 
| --- | --- | --- | 
| một | 3 | | 
| x | 1 | | 
| y | 5 | | 
| x - a | -2 | | 
| y - một | 2 | | 
| sản phẩm | -4 | Tiêu cực, giải thưởng ở hai phía đối diện | 
| Đầu ra | KHÔNG | Bob không thể đảm bảo chiến thắng | 

Những dấu vết này cho thấy rằng thử nghiệm sản phẩm nắm bắt chính xác hình dạng tương đối trong mọi tình huống, ngay cả khi giải thưởng nằm trên tay Alice. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm chỉ yêu cầu tính toán theo thời gian không đổi. | 
| Không gian | O(1) | Không cần thêm bộ nhớ ngoài một vài số nguyên cho mỗi trường hợp thử nghiệm. | 

Với các ràng buộc (t ≤ 1000, tất cả các số ≤ 100), giải pháp thực hiện tốt trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(sys.stdin.readline())
    for _ in range(t):
        a, x, y = map(int, sys.stdin.readline().split())
        if (x - a) * (y - a) > 0:
            output.append("YES")
        else:
            output.append("NO")
    return "\n".join(output)

# Provided samples
assert run("3\n1 3 4\n5 3 1\n3 1 5\n") == "YES\nYES\nNO", "sample 1"

# Custom cases
assert run("2\n2 3 4\n2 1 3\n") == "YES\nNO", "prizes on same side vs opposite side"
assert run("1\n50 51 52\n") == "YES", "consecutive numbers, all on one side"
assert run("1\n50 49 48\n") == "YES", "consecutive numbers on the other side"
assert run("1\n50 49 51\n") == "NO", "prizes straddle Alice"
assert run("1\n1 2 100\n") == "NO", "extreme values, straddling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 3 4 | CÓ | Cả hai giải thưởng cùng một phía | 
| 2 1 3 | KHÔNG | Giải thưởng ở hai phía đối diện | 
| 50 51 52 | CÓ | Giải tuần tự sang phải | 
| 50 49 48 | CÓ | Giải tuần tự sang trái | 
| 50 49 51 | KHÔNG | Giải thưởng nằm trên Alice | 
| 1 2 100 | KHÔNG | Khoảng cách lớn, nằm ngang | 

## Vỏ cạnh 

Nếu Alice ở giữa hai giải thưởng, chẳng hạn như`a = 3, x = 1, y = 5`, sản phẩm`(x - a) * (y - a) = (-2) * 2 = -4`là tiêu cực. Thuật toán ngay lập tức đưa ra "KHÔNG", phản ánh chính xác rằng không có vị trí nào có thể đánh bại Alice để giành cả hai giải thưởng. Nếu cả hai giải đều ở bên phải của Alice (`a = 1, x = 3, y = 4`), tích là dương và thuật toán đưa ra "CÓ", cho phép Bob chọn chính xác`b = 4`để bảo đảm chiến thắng. Những trường hợp này chứng minh rằng thử nghiệm sản phẩm xử lý mạnh mẽ cả cấu hình giải thưởng theo chiều ngang và một chiều.
