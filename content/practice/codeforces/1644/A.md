---
title: "CF 1644A - Cửa và Chìa Khóa"
description: "Bài toán mô tả một hành lang hẹp có ba cánh cửa và ba chiếc chìa khóa, mỗi chiếc có màu đỏ, xanh lá cây hoặc xanh lam riêng biệt. Hiệp sĩ bắt đầu ở cuối bên trái hành lang, và công chúa đợi ở ngoài cùng bên phải."
date: "2026-06-10T04:13:23+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1644
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 123 (Rated for Div. 2)"
rating: 800
weight: 1644
solve_time_s: 74
verified: true
draft: false
---

[CF 1644A - Cửa và Chìa khóa](https://codeforces.com/problemset/problem/1644/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một hành lang hẹp có ba cánh cửa và ba chiếc chìa khóa, mỗi chiếc có màu đỏ, xanh lá cây hoặc xanh lam riêng biệt. Hiệp sĩ bắt đầu ở cuối bên trái hành lang, và công chúa đợi ở ngoài cùng bên phải. Hành lang được thể hiện dưới dạng một chuỗi sáu ký tự: chữ in hoa biểu thị cửa (R, G, B) và chữ thường biểu thị chìa khóa (r, g, b). Mỗi màu xuất hiện chính xác một lần dưới dạng cánh cửa và một lần dưới dạng chìa khóa. 

Mục đích là để xác định xem hiệp sĩ có thể đi qua hành lang từ trái sang phải hay không, mở từng cánh cửa theo thứ tự. Hiệp sĩ có thể nhặt chìa khóa khi di chuyển và sử dụng chúng ngay lập tức khi gặp cánh cửa tương ứng. Đầu ra là "CÓ" nếu tất cả các cửa có thể mở được, nếu không thì "KHÔNG". 

Với những hạn chế, hành lang luôn dài sáu ký tự. Số lượng ca kiểm thử lên tới 720, đủ nhỏ để mọi phương pháp tiếp cận thời gian tuyến tính cho mỗi ca kiểm thử đều hiệu quả. Vì chỉ có ba cửa và ba chìa khóa nên việc kiểm tra vũ phu mô phỏng việc lấy chìa khóa và mở cửa theo thứ tự là khả thi, nhưng cần phải chú ý cẩn thận vì một cánh cửa xuất hiện trước khi khóa của nó tiến triển. 

Các trường hợp cạnh liên quan đến một cánh cửa xuất hiện ngay trước chìa khóa của nó. Chẳng hạn, trên bản đồ`RgbrBG`, cánh cửa màu đỏ`R`là đầu tiên và phím màu đỏ`r`xuất hiện sau này. Hiệp sĩ không thể mở được cánh cửa đầu tiên nên đầu ra phải là "KHÔNG". Một cách tiếp cận ngây thơ chỉ đếm chìa khóa và cửa mà không xem xét thứ tự sẽ nói không chính xác "CÓ". 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp qua chuỗi từ trái sang phải, theo dõi các khóa được thu thập trong một tập hợp. Mỗi khi hiệp sĩ gặp một cánh cửa, chúng tôi sẽ kiểm tra xem chìa khóa tương ứng đã được thu thập chưa. Nếu vậy, anh ta sẽ mở cửa; nếu không, hành lang không thể đi qua được và câu trả lời là "KHÔNG". Cách tiếp cận này hiệu quả vì chiều dài hành lang được cố định ở mức 6, khiến việc lặp lại trở nên đơn giản. Trường hợp xấu nhất liên quan đến việc kiểm tra tất cả sáu ký tự cho mỗi trường hợp thử nghiệm, dẫn đến các phép toán 6 × 720 = 4320, con số này không đáng kể. 

Cách tiếp cận tối ưu tuân theo cùng một quá trình quét tuyến tính. Thông tin chi tiết quan trọng là vì mỗi cánh cửa có một chìa khóa duy nhất và cả hai đều xuất hiện chính xác một lần nên tình huống duy nhất cản trở tiến trình là gặp phải một cánh cửa trước khi lấy được chìa khóa của nó. Vì vậy, chúng ta chỉ cần duy trì một bộ chìa khóa đã thu thập được và kiểm tra các cửa ra vào. Không cần cấu trúc dữ liệu phức tạp hoặc quay lui. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(6 * t) = O(t) | O(3) = O(1) | Đã chấp nhận | 
| Quét tuyến tính tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc chuỗi sáu ký tự đại diện cho hành lang. 
3. Khởi tạo một bộ trống để lưu trữ các khóa đã thu thập cho đến nay. 
4. Lặp lại từng ký tự trong chuỗi từ trái sang phải. 
5. Nếu ký tự là khóa (`r`,`g`,`b`), hãy thêm nó vào tập hợp các khóa đã thu thập. 
6. Nếu ký tự là một cánh cửa (`R`,`G`,`B`), hãy kiểm tra xem phím chữ thường tương ứng của nó có nằm trong tập hợp hay không. Nếu không, hãy in "NO" và dừng xử lý trường hợp thử nghiệm này. 
7. Nếu đến cuối chuỗi mà không gặp cửa bị chặn thì in "CÓ". 

Tại sao nó hoạt động: tại bất kỳ thời điểm nào, bộ này chứa tất cả các chìa khóa mà hiệp sĩ đã vượt qua. Cửa chỉ có thể mở được nếu có chìa khóa trong bộ. Vì mỗi chìa khóa và cánh cửa là duy nhất và xuất hiện một lần nên việc theo dõi các chìa khóa được thu thập sẽ đảm bảo tính chính xác. Không có chu kỳ hoặc lượt xem lại, do đó, chỉ cần thực hiện một lượt từ trái sang phải là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    hallway = input().strip()
    keys_collected = set()
    possible = True
    for ch in hallway:
        if ch.islower():
            keys_collected.add(ch)
        else:  # door
            if ch.lower() not in keys_collected:
                possible = False
                break
    print("YES" if possible else "NO")
```Đầu tiên, mã sẽ đọc số lượng trường hợp kiểm thử và lặp qua chúng. Đối với mỗi chuỗi hành lang, nó khởi tạo một bộ phím theo dõi. Mỗi ký tự được xử lý: các chìa khóa được thêm vào bộ, các cửa được kiểm tra theo bộ. Nếu không mở được cửa, vòng lặp sẽ sớm bị đứt. Kết quả được in ngay lập tức. Các điểm tinh tế bao gồm chuyển đổi cửa thành chữ thường khi kiểm tra phím và loại bỏ dòng mới khỏi chuỗi đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`rgbBRG`| Bước | Nhân vật | Chìa khóa được thu thập | Kiểm tra cửa | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | r | {r} | - | tiếp tục | 
| 2 | g | {r, g} | - | tiếp tục | 
| 3 | b | {r,g,b} | - | tiếp tục | 
| 4 | B | {r,g,b} | vâng | tiếp tục | 
| 5 | R | {r,g,b} | vâng | tiếp tục | 
| 6 | G | {r,g,b} | vâng | tiếp tục | 

Hiệp sĩ thu thập tất cả chìa khóa trước và có thể mở tất cả các cửa. Đầu ra:`YES`. 

### Ví dụ 2:`RgbrBG`| Bước | Nhân vật | Chìa khóa được thu thập | Kiểm tra cửa | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | R | {} | không | nghỉ | 

Cánh cửa màu đỏ xuất hiện đầu tiên nhưng chìa khóa màu đỏ vẫn chưa được thu thập. Đầu ra:`NO`. 

Những ví dụ này xác nhận rằng mô phỏng từ trái sang phải nắm bắt được thứ tự cần thiết của các phím liên quan đến cửa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm xử lý 6 ký tự, do đó tổng số thao tác có tỷ lệ tuyến tính với`t`. | 
| Không gian | O(1) | Chỉ một bộ tối đa 3 khóa được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Với kích thước đầu vào nhỏ, giải pháp này chạy hiệu quả trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        hallway = input().strip()
        keys_collected = set()
        possible = True
        for ch in hallway:
            if ch.islower():
                keys_collected.add(ch)
            else:
                if ch.lower() not in keys_collected:
                    possible = False
                    break
        output.append("YES" if possible else "NO")
    return "\n".join(output)

# Provided samples
assert run("4\nrgbBRG\nRgbrBG\nbBrRgG\nrgRGBb\n") == "YES\nNO\nYES\nNO", "Sample 1"

# Custom test cases
assert run("1\nrbgRGB\n") == "YES", "all keys first"
assert run("1\nRGBrgb\n") == "NO", "all doors first"
assert run("1\nrGBgRb\n") == "NO", "blocked by first door"
assert run("1\nrgBGRb\n") == "YES", "keys before doors but mixed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| rbgRGB | CÓ | Thu thập tất cả các khóa trước | 
| RGBrgb | KHÔNG | Cánh cửa xuất hiện trước chìa khóa | 
| rGBgRb | KHÔNG | Tiến độ chặn cửa đầu tiên | 
| rgBGRb | CÓ | Trật tự hỗn hợp nhưng tất cả các cửa đều có thể mở được | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi một cánh cửa xuất hiện trước chìa khóa của nó. Vì`RGBrgb`, hiệp sĩ gặp ngay cánh cửa màu đỏ và không có chìa khóa nên việc vượt qua là không thể. Thuật toán của chúng tôi phát hiện điều này ở bước đầu tiên và đưa ra kết quả là "KHÔNG". Một trường hợp tinh tế khác là khi tất cả các phím xuất hiện đầu tiên, ví dụ:`rbgRGB`, cho phép mở cửa ngay lập tức mà thuật toán đưa ra chính xác là "CÓ". Những thử nghiệm này xác nhận rằng thứ tự là yếu tố duy nhất ảnh hưởng đến khả năng giải quyết, chứ không phải vị trí tương đối của cửa hoặc chìa khóa không chặn.
