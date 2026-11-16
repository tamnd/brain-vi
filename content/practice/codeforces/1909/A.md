---
title: "CF 1909A - Nút riêng biệt"
description: "Chúng ta được yêu cầu di chuyển trên một lưới vô hạn bắt đầu từ điểm gốc. Chúng ta có bốn bước di chuyển có thể tương ứng với bốn hướng chính: lên, xuống, trái và phải."
date: "2026-06-08T20:28:13+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1909
codeforces_index: "A"
codeforces_contest_name: "Pinely Round 3 (Div. 1 + Div. 2)"
rating: 800
weight: 1909
solve_time_s: 96
verified: true
draft: false
---

[CF 1909A - Nút riêng biệt](https://codeforces.com/problemset/problem/1909/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu di chuyển trên một lưới vô hạn bắt đầu từ điểm gốc. Chúng ta có bốn bước di chuyển có thể tương ứng với bốn hướng chính: lên, xuống, trái và phải. Mỗi lần di chuyển được kích hoạt bằng một nút riêng biệt trên bộ điều khiển, nhưng bộ điều khiển rất dễ vỡ: nếu chúng ta nhấn cả bốn nút ít nhất một lần trong suốt hành trình, nó sẽ bị hỏng. Do đó, chúng tôi bị giới hạn sử dụng tối đa ba hướng riêng biệt để đạt được tất cả các điểm đã cho. Mỗi trường hợp thử nghiệm cung cấp một tập hợp các điểm có tọa độ nguyên và chúng ta cần xác định xem liệu có thể truy cập tất cả các điểm đó mà không cần nhấn cả bốn nút hay không. 

Những hạn chế là vừa phải. Có thể có tới 100 điểm cho mỗi trường hợp thử nghiệm và tối đa 1000 trường hợp thử nghiệm. Mỗi tọa độ đều nhỏ, từ -100 đến 100, cho phép chúng tôi thực hiện quét tọa độ đơn giản mà không phải lo lắng về sự kém hiệu quả. Thách thức chính không phải là hiệu suất tính toán mà là lý luận hình học: chúng ta cần tìm ra hướng nào là cần thiết để đạt được tất cả các điểm. 

Một sai lầm ngây thơ là cố gắng truy cập các điểm theo thứ tự tùy ý và theo dõi các lần nhấn nút chính xác. Ví dụ: nếu các điểm trải dài trên tất cả bốn góc phần tư so với điểm gốc, một cách tiếp cận bất cẩn có thể thử một đường đi truy cập từng góc phần tư một cách tuần tự và nhầm tưởng rằng điều đó luôn có thể xảy ra. Ví dụ, với điểm`(1,1)`,`(-1,-1)`,`(1,-1)`, Và`(-1,1)`, đường đi nào cũng phải sử dụng cả 4 hướng nên câu trả lời đúng là "KHÔNG". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của các điểm và xem liệu có tồn tại thứ tự truy cập chúng sử dụng tối đa ba hướng hay không. Điều này là không thực tế vì sự tăng trưởng giai thừa của các hoán vị sẽ bùng nổ ngay cả đối với`n=10`. Về nguyên tắc nó đúng nhưng không khả thi đối với những hạn chế của chúng tôi. 

Quan sát chính là chúng ta không cần mô phỏng đường dẫn hoặc thứ tự. Góc phần tư của mỗi điểm so với gốc tọa độ sẽ xác định hướng nào là cần thiết. Chúng ta có thể phân loại từng điểm theo tọa độ x và y của nó: 

- Để đến được những điểm có x > 0 thì ta phải di chuyển được sang phải. 
- Để đến được những điểm có x < 0, ta phải di chuyển được sang trái. 
- Muốn đến được những điểm y > 0 thì ta phải tiến lên được. 
- Để đến được những điểm có y < 0 thì ta phải di chuyển được xuống dưới. 

Điều này mang lại bốn hướng tiềm năng, tương ứng với bốn nút. Nếu cần phải có cả bốn hướng để đến được điểm, bộ điều khiển sẽ bị hỏng. Nếu không thì nó an toàn. 

Do đó, đối với mỗi trường hợp thử nghiệm, chúng tôi kiểm tra xem có đồng thời có các điểm dương x, âm x, dương y và âm y hay không. Nếu cả bốn đều tồn tại, câu trả lời là "KHÔNG". Nếu không thì là "CÓ". This reduces the problem to simple scanning of coordinates.

 | Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, khởi tạo bốn cờ:`need_right`,`need_left`,`need_up`,`need_down`ĐẾN`False`. Chúng thể hiện liệu chúng ta có phải nhấn nút tương ứng để đến bất kỳ điểm nào hay không. 
2. Lặp lại tất cả các điểm trong trường hợp thử nghiệm. Đối với mỗi điểm`(x, y)`, cập nhật các cờ: 

- Nếu`x > 0`, bộ`need_right = True`. 
- Nếu như`x < 0`, bộ`need_left = True`. 
- Nếu như`y > 0`, bộ`need_up = True`. 
- Nếu như`y < 0`, bộ`need_down = True`. 
3. Sau khi xử lý tất cả các điểm, hãy kiểm tra xem tất cả bốn cờ có`True`. Nếu vậy, xuất ra "KHÔNG" vì việc truy cập tất cả các điểm cần có cả bốn nút. Nếu không thì xuất ra "CÓ". 
4. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: các lá cờ nắm bắt được tập hợp hướng dẫn tối thiểu cần thiết để đến được tất cả các điểm. Nếu bất kỳ nút nào không cần thiết, bộ điều khiển sẽ không bị hỏng. Vì chúng tôi chỉ đếm những nút nào được yêu cầu nên thứ tự thực tế của các điểm truy cập không quan trọng. Giải pháp trả lời đúng là "CÓ" khi và chỉ khi cần tối đa ba nút. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    need_right = need_left = need_up = need_down = False
    for _ in range(n):
        x, y = map(int, input().split())
        if x > 0:
            need_right = True
        if x < 0:
            need_left = True
        if y > 0:
            need_up = True
        if y < 0:
            need_down = True
    if need_right and need_left and need_up and need_down:
        print("NO")
    else:
        print("YES")
```Mã tuân theo thuật toán từng bước. Các cờ được cập nhật độc lập cho từng điểm. Chúng tôi tránh lưu trữ điểm vì chúng tôi chỉ cần thông tin chỉ đường. Điều kiện biên nơi các điểm nằm trên trục được xử lý tự động vì`x=0`hoặc`y=0`không yêu cầu bất kỳ chuyển động ngang hoặc dọc. sử dụng`sys.stdin.readline`đảm bảo đầu vào nhanh chóng cho số lượng lớn các trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp thử nghiệm mẫu đầu tiên: 

| Bước | Điểm`(x, y)`| Cờ sau khi cập nhật | 
| --- | --- | --- | 
| 1 | (1, -1) | right=True, left=False, up=False, down=True | 
| 2 | (0, 0) | right=True, left=False, up=False, down=True | 
| 3 | (1, -1) | right=True, left=False, up=False, down=True | 

Không có lá cờ nào đồng thời là tất cả`True`, vì vậy đầu ra là "CÓ". Dấu vết xác nhận rằng chỉ cần hai nút: phải và xuống. 

Hãy xem xét mẫu thứ ba: 

| Bước | Điểm`(x, y)`| Cờ sau khi cập nhật | 
| --- | --- | --- | 
| 1 | (1,1) | phải=Đúng, trái=Sai, lên=Đúng, xuống=Sai | 
| 2 | (-1,-1) | right=True, left=True, up=True, down=True | 
| 3 | (1,-1) | right=True, left=True, up=True, down=True | 
| 4 | (-1,1) | right=True, left=True, up=True, down=True | 

Tất cả bốn cờ đều đúng, vì vậy đầu ra là "KHÔNG". Dấu vết chứng tỏ rằng các điểm trong cả bốn góc phần tư đều yêu cầu tất cả bốn nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp kiểm thử yêu cầu quét tất cả n điểm và t ≤ 1000, n ≤ 100 | 
| Không gian | O(1) | Chỉ có bốn cờ boolean được lưu trữ cho mỗi trường hợp thử nghiệm | 

Với các giá trị tối đa nhỏ của t và n, tổng số thao tác tối đa là 1000 * 100 = 100.000, dễ dàng nằm gọn trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        need_right = need_left = need_up = need_down = False
        for _ in range(n):
            x, y = map(int, input().split())
            if x > 0:
                need_right = True
            if x < 0:
                need_left = True
            if y > 0:
                need_up = True
            if y < 0:
                need_down = True
        if need_right and need_left and need_up and need_down:
            print("NO")
        else:
            print("YES")
    return output.getvalue().strip()

# Provided samples
assert run("6\n3\n1 -1\n0 0\n1 -1\n4\n-3 -2\n-3 -1\n-3 0\n-3 1\n4\n1 1\n-1 -1\n1 -1\n-1 1\n6\n-4 14\n-9 -13\n-14 5\n14 15\n-8 -4\n19 9\n6\n82 64\n39 91\n3 46\n87 83\n74 21\n7 25\n1\n100 -100") == "YES\nYES\nNO\nNO\nYES\nYES"

# Custom cases
assert run("1\n1\n0 0") == "YES", "single origin point"
assert run("1\n2\n0 5\n0 -5") == "YES", "vertical line requires up/down only"
assert run("1\n2\n5 0\n-5 0") == "YES", "horizontal line requires left/right only"
assert run("1\n4\n1 1\n1 -1\n-1 1\n-1 -1") == "NO", "points in all four quadrants"
assert run("1\n3\n0 1\n1 0\n0 -1") == "YES", "three directions only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 điểm tại điểm xuất phát | CÓ | Không cần chuyển động | 
| Điểm dọc | CÓ | Chỉ sử dụng hai hướng | 
| Điểm ngang | CÓ | Chỉ sử dụng hai hướng | 
| Bốn góc phần tư | KHÔNG | Yêu cầu cả bốn phương | 
| Ba hướng | CÓ | Cần chính xác ba nút, hợp lệ | 

## Vỏ cạnh 

Nếu tất cả các điểm
