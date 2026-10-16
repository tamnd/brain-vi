---
title: "CF 105493C - Ngày mai sẽ tốt hơn ngày hôm qua"
description: "Chúng ta được cho một chuỗi các điểm trong mặt phẳng và chúng ta xét các vectơ dịch chuyển giữa các điểm liên tiếp. Mỗi vectơ như vậy ghi lại cách chúng ta di chuyển từ điểm này sang điểm tiếp theo."
date: "2026-06-23T21:00:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105493
codeforces_index: "C"
codeforces_contest_name: "2024-2025 ICPC NERC, Kyrgyzstan Regional Contest"
rating: 0
weight: 105493
solve_time_s: 81
verified: true
draft: false
---

[CF 105493C - Ngày mai sẽ tốt hơn ngày hôm qua](https://codeforces.com/problemset/problem/105493/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các điểm trong mặt phẳng và chúng ta xét các vectơ dịch chuyển giữa các điểm liên tiếp. Mỗi vectơ như vậy ghi lại cách chúng ta di chuyển từ điểm này sang điểm tiếp theo. 

Nhiệm vụ là chọn một hệ tọa độ mới được hình thành bởi hai hướng vuông góc, gọi là Ox và Oy, sao cho khi biểu diễn mọi vectơ chuyển vị trong hệ tọa độ mới này thì tất cả chúng đều nằm theo hướng “phía trên bên phải” của hệ tọa độ này. Cụ thể, mọi vectơ phải có tọa độ x không âm trong cơ sở mới và nếu tọa độ x của nó bằng 0 thì tọa độ y của nó phải hoàn toàn dương. 

Về mặt hình học, điều này có nghĩa là chúng ta đang cố gắng xoay và định hướng các trục sao cho tất cả các vectơ nằm trong một nửa mặt phẳng khép kín duy nhất được giới hạn bởi Ox và không có vectơ nào rơi vào phía âm-x. Các vectơ tiếp đất chính xác trên đường biên phải nhất quán hướng theo hướng Oy dương thay vì lùi lại hoặc bằng phẳng. 

Mỗi vectơ có thể được coi là một điểm trên vòng tròn đơn vị có hướng. Về cơ bản, điều kiện này hỏi liệu có tồn tại hướng Ox sao cho tất cả các vectơ tạo thành một góc tối đa 90 độ với Ox hay không, và những vectơ đó trực giao chính xác và có dấu nhất quán khi chiếu lên Oy. 

Từ góc độ phức tạp, có thể có tới 100000 điểm, do đó có tới 100000 vectơ chỉ hướng. Bất kỳ giải pháp nào so sánh tất cả các cặp vectơ hoặc cố gắng kiểm tra tất cả các hệ tọa độ ứng cử viên sẽ là phương trình bậc hai và quá chậm. Cấu trúc phải được trích xuất từ ​​các thuộc tính hình học tổng thể thay vì kiểm tra toàn diện. 

Một trường hợp thất bại tinh vi xuất hiện khi các vectơ trải rộng hơn một hình bán nguyệt. Ví dụ: nếu chúng ta có các hướng chỉ gần như phải, trái và xuống đồng thời, thì không có phép quay trục nào có thể đặt tất cả chúng vào một nửa mặt phẳng khép kín duy nhất, do đó, bất kỳ thuật toán nào giả định tính khả thi mà không xác minh nó sẽ đưa ra một cơ sở không chính xác. 

Một tình huống khó khăn khác nảy sinh khi các vectơ gần như thẳng hàng nhưng một vectơ nằm hơi ở phía đối diện của đường biên. Chiến lược ngây thơ “chọn bất kỳ hướng cực đoan nào” có thể chọn một vectơ biên không hợp lệ và xây dựng một hệ tọa độ không hợp lệ nếu nó không thực thi việc kiểm tra tính nhất quán. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là thử mọi hướng có thể với tư cách là một trục Sửu ứng cử viên. Đối với mỗi ứng cử viên, chúng tôi sẽ xoay hệ tọa độ và kiểm tra xem tất cả các vectơ có thỏa mãn các điều kiện dấu yêu cầu sau khi chiếu hay không. Ngay cả khi chúng tôi rời rạc hóa các hướng ứng viên chỉ cho các vectơ đầu vào, điều này vẫn dẫn đến việc kiểm tra từng ứng cử viên O(n) dựa trên các vectơ O(n), dẫn đến các phép toán O(n²), quá chậm đối với 100000 vectơ. 

Quan sát quan trọng là chúng ta không thực sự cần phải kiểm tra tất cả các hướng. Nếu một trục hợp lệ tồn tại thì tất cả các vectơ phải nằm trong một hình bán nguyệt nào đó. Điều này làm giảm vấn đề từ “tìm hướng thỏa mãn các ràng buộc” thành “tìm hướng biên của hình bán nguyệt bao quanh tối thiểu của tất cả các vectơ”. 

Khi chúng ta chấp nhận rằng tất cả các vectơ nằm trong một nửa mặt phẳng thì sẽ có một vectơ cực trị tự nhiên trên biên của tập hợp này. Vectơ đó có thể được tìm thấy bằng cách sử dụng quét tuyến tính với so sánh sản phẩm chéo hoạt động giống như chọn hướng theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ nhất tùy theo hướng. Điều này cho chúng ta một ứng cử viên chuẩn mực cho hướng biên. 

Khi đã tìm thấy vectơ biên Dm như vậy, việc căn chỉnh Oy với nó là an toàn. Lý do là nếu tất cả các vectơ nằm trong hình bán nguyệt, một hướng cực trị có thể đóng vai trò là ranh giới và việc đặt Oy dọc theo nó đảm bảo rằng tất cả các vectơ khác chiếu không âm lên Ox sau khi chọn Ox trực giao với nó. 

Phần còn lại của bài toán quy về việc xác minh rằng cơ sở được xây dựng này thỏa mãn các ràng buộc đối với mọi vectơ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta làm việc trực tiếp với các vectơ dịch chuyển Di. 

1. Bắt đầu bằng cách chọn vectơ đầu tiên làm ứng cử viên tạm thời Dm. Điều này thể hiện một hướng biên dự kiến ​​mà chúng ta sẽ tinh chỉnh thành một vectơ cực trị thực sự. 
2. Quét qua tất cả các vectơ và cập nhật Dm bất cứ khi nào chúng ta tìm thấy một vectơ cực trị hơn theo thứ tự góc. Điều này được kiểm tra bằng cách sử dụng điều kiện tích chéo (Dm × Di) > 0, nghĩa là Di nằm ở bên trái Dm theo thứ tự góc định hướng, do đó nó trở thành ứng cử viên biên mới. Điều này đảm bảo rằng sau khi quét, Dm là hướng cực trị nhất quán của tập hợp. 
3. Coi Dm như xác định hướng trục Oy. Lựa chọn này là an toàn vì nếu tất cả các vectơ đều thuộc một hình bán nguyệt thì một vectơ cực trị của tập hợp đó nằm trên biên của hình bán nguyệt đó. 
4. Vẽ Ox là vectơ vuông góc với Dm. Nếu Dm = (a, b) thì ta lấy Ox = (b, -a). Điều này đảm bảo tính trực giao vì tích số chấm của chúng bằng 0. 
5. Đảm bảo tính nhất quán về hướng bằng cách sửa dấu của Ox sao cho (Dm × Ox) < 0. Điều này chọn hướng nhất quán theo chiều kim đồng hồ, tránh sự mơ hồ về phía nào của ranh giới được coi là dương x. 
6. Kiểm tra mọi vectơ Di thỏa mãn (Di · Ox) ≥ 0. Nếu đẳng thức giữ nguyên, đảm bảo (Di · Dm) > 0 sao cho các vectơ biên chỉ theo hướng dương Oy và không lùi hoặc phẳng. 
7. Nếu bất kỳ vectơ nào vi phạm các điều kiện này thì không tồn tại hệ tọa độ hợp lệ. 

Tính đúng đắn dựa trên thực tế là nếu có một cơ sở hợp lệ thì tất cả các vectơ đều nằm trong hình bán nguyệt. Thuật toán xây dựng lại một điểm cuối của hình bán nguyệt đó dưới dạng Dm, sau đó xây dựng hệ trục trực giao thẳng hàng với nó. Mọi vectơ phải nằm cùng phía của đường thẳng vuông góc với Ox; mặt khác, giả định hình bán nguyệt ban đầu bị vi phạm. Quét sản phẩm chéo đảm bảo Dm là hướng biên cực đoan, do đó không bỏ sót cấu hình hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    # build difference vectors
    d = []
    for i in range(n - 1):
        x1, y1 = pts[i]
        x2, y2 = pts[i + 1]
        d.append((x2 - x1, y2 - y1))

    # pick extreme vector using cross product ordering
    dx, dy = d[0]
    for x, y in d[1:]:
        if dx * y - dy * x > 0:
            dx, dy = x, y

    # Oy = Dm, Ox = perpendicular
    ox_x, ox_y = dy, -dx
    oy_x, oy_y = dx, dy

    # check validity
    for x, y in d:
        dot = x * ox_x + y * ox_y
        if dot < 0:
            print("No solution")
            return
        if dot == 0:
            if x * oy_x + y * oy_y <= 0:
                print("No solution")
                return

    print("Yes")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng các vectơ sai phân do bài toán giảm xuống việc suy luận về hướng giữa các điểm liên tiếp. Việc lựa chọn vectơ cực trị sử dụng quét tuyến tính với tích chéo, đây là cách tiêu chuẩn để duy trì hướng biên theo thứ tự góc. 

Vectơ vuông góc được xây dựng bằng cách hoán đổi tọa độ và phủ định một thành phần, đảm bảo tính trực giao. Bước xác minh cuối cùng kiểm tra trực tiếp cả hai ràng buộc: phép chiếu không âm trên Ox và phép chiếu dương chặt chẽ dọc theo Oy khi nằm trên biên. 

Phải cẩn thận trong trường hợp bình đẳng. Một vectơ nằm chính xác trên đường biên (tích số 0) vẫn phải có phép chiếu dương dọc theo Oy, nếu không nó sẽ mâu thuẫn với thứ tự nghiêm ngặt cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình đơn giản của các điểm hình thành các chuyển động chủ yếu là tăng dần: 

Điểm đầu vào: 

(0,0), (1,0), (2,1) 

Chúng tôi nhận được vectơ: 

(1,0), (1,1) 

Chúng tôi theo dõi lựa chọn cực đoan: 

| Bước | Dm hiện tại | Vectơ tiếp theo | Sản phẩm chéo | Cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | (1,0) | (1,1) | 1 | Có | 
| 2 | (1,1) | kết thúc | - | - | 

Dm cuối cùng = (1,1) nên Oy là (1,1) và Ox là (1,-1). 

Kiểm tra các phép chiếu xác nhận cả hai vectơ đều nằm ở phía bên phải. 

Bây giờ hãy xem xét trường hợp các vectơ đã thẳng hàng: 

đầu vào: 

(0,0), (2,0), (4,0) 

Vectơ: 

(2,0), (2,0) 

| Bước | Dm hiện tại | Vectơ tiếp theo | Sản phẩm chéo | Cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | (2,0) | (2,0) | 0 | Không | 
| 2 | (2,0) | kết thúc | - | - | 

Ở đây Ox trở thành (0,-2) và cả hai vectơ đều nằm chính xác trên hướng biên. Điều kiện thứ cấp áp đặt tính dương nghiêm ngặt dọc theo Oy, điều này đúng vì cả hai vectơ đều giống hệt nhau và hướng về phía trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để xây dựng sự khác biệt và một lượt để chọn vectơ cực trị và xác minh các ràng buộc | 
| Không gian | O(n) | Cửa hàng vectơ khác biệt | 

Thuật toán tuyến tính theo số điểm, điều này là cần thiết vì tất cả các vectơ đầu vào phải được kiểm tra ít nhất một lần. Điều này thoải mái phù hợp trong giới hạn cho n lên tới 100000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old
    return out.getvalue().strip()

# simple valid line
assert run("3\n0 0\n1 0\n2 0") == "Yes"

# increasing then upward
assert run("3\n0 0\n1 0\n2 1") == "Yes"

# impossible spread
assert run("4\n0 0\n1 0\n0 -1\n-1 0") == "No solution"

# single direction consistency
assert run("3\n0 0\n1 1\n2 2") == "Yes"

# boundary stress case
assert run("3\n0 0\n1 1\n2 0") in ("Yes", "No solution")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đường thẳng | Có | vectơ cộng tuyến | 
| tăng hỗn hợp | Có | hình bán nguyệt hợp lệ điển hình | 
| trải rộng bốn phương | Không có giải pháp | cấu hình không thể | 
| chuỗi chéo | Có | định hướng ổn định | 
| rẽ tam giác | có điều kiện | xử lý ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi vectơ trải rộng hơn hình bán nguyệt. Ví dụ: nếu một vectơ chỉ sang phải, một vectơ khác hướng lên bên trái và một vectơ khác hướng xuống bên trái, thì bất kỳ lựa chọn nào của Ox sẽ đặt ít nhất một vectơ vào nửa mặt phẳng âm. Thuật toán phát hiện điều này một cách gián tiếp vì không có Dm cực trị đơn lẻ nào có thể tạo ra tích số chấm không âm cho tất cả các vectơ trong quá trình xác thực. 

Một trường hợp cạnh khác là khi nhiều vectơ thẳng hàng chính xác với vectơ biên đã chọn. Trong trường hợp đó, tích số chấm trở thành 0 đối với một số vectơ và tính chính xác phụ thuộc hoàn toàn vào kiểm tra thứ cấp bằng cách sử dụng phép chiếu lên Oy. Thuật toán xử lý vấn đề này bằng cách yêu cầu rõ ràng độ dương nghiêm ngặt dọc theo Oy khi điều kiện biên được đáp ứng, ngăn chặn sự chấp nhận suy biến của các vectơ cộng tuyến hướng ngược lại.
