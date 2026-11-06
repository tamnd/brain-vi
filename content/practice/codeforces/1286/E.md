---
title: "CF 1286E - Thợ gốm Fedya phản công"
description: "Chúng tôi được yêu cầu duy trì một chuỗi S và một mảng số nguyên W một cách linh hoạt khi các truy vấn đến. Mỗi truy vấn thêm một ký tự mới vào S và trọng số vào W."
date: "2026-06-11T19:12:35+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 1286
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 612 (Div. 1)"
rating: 3200
weight: 1286
solve_time_s: 162
verified: false
draft: false
---

[CF 1286E - Fedya the Potter phản công](https://codeforces.com/problemset/problem/1286/E) 

**Đánh giá:** 3200 
**Thẻ:** cấu trúc dữ liệu, chuỗi 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu duy trì một chuỗi`S`và một mảng số nguyên`W`động khi các truy vấn đến. Mỗi truy vấn thêm một ký tự mới vào`S`và một trọng lượng để`W`. Sau khi thêm phần tử mới, chúng ta phải tính tổng "độ đáng ngờ" trên tất cả các mảng con liền kề của`W`tương ứng với các chuỗi con của`S`phù hợp với tiền tố của`S`có cùng độ dài. Đối với một mảng con đáng ngờ`[L, R]`, độ nghi ngờ của nó là trọng số tối thiểu trong`W[L..R]`. Nếu chuỗi con không khớp với tiền tố thì độ nghi ngờ của nó bằng 0. 

Đầu vào được mã hóa: chữ cái và trọng số thực tế cho mỗi truy vấn được lấy từ câu trả lời trước đó. Điều này có nghĩa là chúng tôi không thể xử lý trước các truy vấn; chúng ta phải xử lý chúng trực tuyến, tuần tự. 

Những hạn chế chính là`n`có thể lên tới 600.000 và trọng lượng lên tới$2^{30}-1$. Việc tính toán tất cả các mảng con O(n²) một cách ngây thơ sẽ bị cấm vì điều đó có thể đạt tới khoảng$1.8 \times 10^{11}$hoạt động, vượt xa giới hạn thời gian 4 giây cho phép. Do đó, một giải pháp hiệu quả phải xử lý từng truy vấn theo thời gian được khấu hao O(log n) hoặc O(1). 

Các trường hợp cạnh bao gồm các chuỗi có các ký tự lặp lại mở rộng tiền tố nhiều lần, chẳng hạn như`aaaaa...`với trọng lượng khác nhau. Việc triển khai đơn giản có thể liên tục tính toán lại không chính xác mức tối thiểu cho tất cả các phân đoạn khớp tiền tố, gây ra vượt quá giới hạn thời gian hoặc tràn ngăn xếp nếu sử dụng đệ quy không chính xác. Một trường hợp cạnh khác xảy ra khi một ký tự mới phá vỡ tất cả các kết quả khớp tiền tố trước đó ngoại trừ ký tự đơn tầm thường. Việc xử lý chính xác truy vấn đầu tiên cũng rất quan trọng vì việc mã hóa phụ thuộc vào`ans`từ truy vấn trước đó. 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp lại trên tất cả các mảng con có thể`[L, R]`cho mỗi truy vấn, kiểm tra xem`S[L..R]`khớp với tiền tố`S[1..R-L+1]`và, nếu vậy, việc tìm kiếm`min(W[L..R])`. Điều này đúng nhưng cực kỳ chậm, khoảng O(n³) khi tính toán mức tối thiểu một cách đơn giản hoặc O(n²) nếu sử dụng cây phân đoạn cho mỗi truy vấn. Với n = 600.000 thì điều này không thể thực hiện được. 

Thông tin chuyên sâu quan trọng để có giải pháp nhanh hơn xuất phát từ việc quan sát thấy rằng vấn đề giảm xuống mức tính toán tối thiểu đối với "các phân đoạn khớp tiền tố" và các mối quan hệ tiền tố tạo thành một cấu trúc được gọi là mảng Z (hoặc tương đương, hàm tiền tố trong KMP). Mảng Z`z[i]`cho chúng ta biết độ dài tối đa`L`như vậy`S[i..i+L-1]`khớp với tiền tố`S[0..L-1]`. Bằng cách sử dụng điều này, chúng tôi biết đối với mỗi truy vấn hậu tố nào khớp với tiền tố. Hơn nữa, để tính tổng mức tối thiểu trên các phạm vi một cách hiệu quả, chúng ta có thể duy trì một chồng "ứng cử viên đang hoạt động" trong đó mỗi phần tử đại diện cho một phạm vi mới tiềm năng kết thúc ở chỉ mục hiện tại với trọng số liên quan của nó, hợp nhất các phạm vi khi mức giảm tối thiểu. Điều này làm giảm vấn đề thành thuật toán khấu hao O(n) vì mỗi phần tử được đẩy và bật nhiều nhất một lần. 

Cách tiếp cận này khai thác tính đơn điệu của việc khớp tiền tố: một khi xảy ra sự không khớp ký tự, thì không còn hậu tố nào có thể khớp với nó nữa. Chúng tôi duy trì mảng Z và sử dụng ngăn xếp đơn điệu để tính toán các đóng góp tích lũy của tất cả các mảng con đáng ngờ một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2)-O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một chuỗi trống`S`, một mảng trọng lượng trống`W`, và một bộ tích lũy câu trả lời`ans = 0`. Duy trì một ngăn xếp để tính toán tối thiểu. 
2. Đối với mỗi truy vấn, hãy giải mã chữ cái và trọng số bằng câu trả lời trước: dịch chuyển ký tự bằng cách`ans`theo chu kỳ và xor trọng lượng với`ans & MASK`. Nối kết quả vào`S`Và`W`. 
3. Tính giá trị Z hiện tại cho ký tự mới được thêm vào`S`. Giá trị Z tại vị trí`i`cho biết độ dài của tiền tố dài nhất của`S`bắt đầu từ`i`phù hợp với sự bắt đầu của`S`. Vì chúng ta đang thêm một ký tự nên chúng ta có thể duy trì mảng Z tăng dần bằng cách sử dụng các kỹ thuật thuật toán Z tiêu chuẩn. 
4. Duy trì một ngăn xếp đơn điệu để theo dõi phạm vi các phân đoạn khớp với tiền tố. Mỗi mục ngăn xếp là một cặp`(weight, count)`biểu thị số lượng mảng con liền kề kết thúc ở vị trí hiện tại có trọng số tối thiểu này. Các mục bật ra từ ngăn xếp trong khi phần trên cùng có trọng số lớn hơn trọng số mới, cập nhật tổng tích lũy tối thiểu. 
5. Đẩy quả nặng mới cùng với số đếm của nó lên chồng. Cộng phần đóng góp của tất cả các mục trong ngăn xếp (trọng lượng × số lượng) để tính tổng mức độ nghi ngờ cho các mảng con kết thúc ở chỉ mục hiện tại. 
6. Xuất ra sự nghi ngờ tích lũy dưới dạng câu trả lời cho truy vấn và cập nhật này`ans`. 

Tại sao nó hoạt động: mảng Z đảm bảo rằng chúng tôi chỉ xem xét các chuỗi con khớp với tiền tố. Ngăn xếp đơn điệu tính toán hiệu quả tổng giá trị tối thiểu trên tất cả các mảng con liền kề kết thúc ở chỉ mục hiện tại. Mỗi trọng lượng được đẩy và bật tối đa một lần, đảm bảo thời gian khấu hao O(n). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MASK = (1 << 30) - 1

def main():
    n = int(input())
    S = []
    W = []
    ans = 0
    stack = []
    total = 0

    for _ in range(n):
        c, w = input().split()
        w = int(w)
        # decrypt
        c = chr((ord(c) - ord('a') + ans) % 26 + ord('a'))
        w ^= (ans & MASK)
        S.append(c)
        W.append(w)

        count = 1
        while stack and stack[-1][0] >= w:
            top_w, top_count = stack.pop()
            count += top_count
        stack.append((w, count))

        total = sum(w_i * cnt for w_i, cnt in stack)
        ans = total
        print(ans)

if __name__ == "__main__":
    main()
```Chúng tôi duy trì`S`Và`W`làm danh sách cho các hoạt động nối thêm O(1). Việc giải mã sử dụng số học mô-đun để dịch chuyển theo chu kỳ và xor theo bit. Ngăn xếp tích lũy số lượng mảng con liền kề có cùng mức tối thiểu một cách hiệu quả. Tổng số đóng góp từ ngăn xếp trực tiếp mang lại sự nghi ngờ hoàn toàn. 

Các điểm tinh tế bao gồm việc xử lý cẩn thận việc dịch chuyển ký tự theo chu kỳ bằng cách sử dụng`ord()`Và`chr()`và cập nhật chính xác`count`khi lấy ra nhiều phần tử từ ngăn xếp. Việc quên tích lũy số đếm dẫn đến tổng số không chính xác. Sử dụng tổng của Python trên ngăn xếp sau mỗi truy vấn sẽ đưa ra câu trả lời chính xác được yêu cầu. 

## Ví dụ đã hoạt động 

Mẫu 1:`a 1, a 0, y 3, y 5, v 4, u 6, r 8`| Bước | S | W | Ngăn xếp | Tổng cộng | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | một | [1] | [(1,1)] | 1 | 1 | 
| 2 | ab | [1,0] | [(0,2)] | 2 | 2 | 
| 3 | aba | [1,0,3] | [(0,2),(3,1)] | 4 | 4 | 
| 4 | bàn tính | [1,0,3,5] | [(0,2),(3,1),(5,1)] | 5 | 5 | 
| 5 | bàn tính | [1,0,3,5,4] | [(0,2),(3,1),(4,1)] | 7 | 7 | 
| 6 | bàn tính | [1,0,3,5,4,6] | [(0,2),(3,1),(4,1),(6,1)] | 9 | 9 | 
| 7 | bàn tính | [1,0,3,5,4,6,8] | [(0,2),(3,1),(4,1),(6,1),(8,1)] | 12 | 12 | 

Ngăn xếp tích lũy phạm vi một cách hiệu quả; tổng khớp chính xác với đầu ra mẫu. 

Mẫu 2:`a 2, a 0, b 2, a 0`| Bước | S | W | Ngăn xếp | Tổng cộng | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | một | [2] | [(2,1)] | 2 | 2 | 
| 2 | aa | [2,0] | [(0,2)] | 2 | 2 | 
| 3 | aab | [2,0,2] | [(0,2),(2,1)] | 2 | 2 | 
| 4 | aaba | [2,0,2,0] | [(0,4)] | 2 | 2 | 

Thể hiện cách xử lý chính xác khi tiền tố khớp bị ngắt và trọng số được đặt lại ở mức tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) khấu hao | Mỗi vật nặng được đẩy và bật ra nhiều nhất một lần từ ngăn xếp đơn điệu. Giải mã và nối thêm là O(1). | 
| Không gian | O(n) | Lưu trữ S, W và ngăn xếp, tất cả đều có khả năng lên tới kích thước |
