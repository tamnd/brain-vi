---
title: "CF 253A - Bé Trai và Bé Gái"
description: "Chúng ta được yêu cầu sắp xếp một chuỗi gồm hai loại đồ vật, bé trai và bé gái, thành một dòng duy nhất. Sự tự do duy nhất chúng ta có là trật tự."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 253
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 154 (Div. 2)"
rating: 1100
weight: 253
solve_time_s: 67
verified: true
draft: false
---

[CF 253A - Nam và Nữ](https://codeforces.com/problemset/problem/253/A) 

**Đánh giá:** 1100 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu sắp xếp một chuỗi gồm hai loại đồ vật, bé trai và bé gái, thành một dòng duy nhất. Sự tự do duy nhất chúng ta có là trật tự. Chất lượng của sự sắp xếp được đo lường bằng tần suất các vị trí liền kề có giới tính khác nhau, tức là số lần một cậu bé ngồi cạnh một cô gái. 

Mục tiêu là xây dựng bất kỳ thứ tự hợp lệ nào làm cho các “ranh giới xen kẽ” này xảy ra thường xuyên nhất có thể, đồng thời tôn trọng rằng chúng ta phải sử dụng chính xác tất cả các bé trai và bé gái nhất định. 

Đầu vào cho hai số nguyên nhỏ, số bé trai và số bé gái. Đầu ra là một chuỗi có tổng chiều dài đó, bao gồm hai ký hiệu đại diện cho hai giới tính. 

Các ràng buộc rất nhỏ, với cả hai số lượng tối đa là 100. Điều đó ngay lập tức loại trừ mọi nhu cầu tối ưu hóa nhiều. Ngay cả một giải pháp thử tất cả các hoán vị cũng sẽ quá chậm, vì tổng số cách sắp xếp tăng theo giai thừa và trở nên khổng lồ ngay cả đối với các kích thước vừa phải như 10 hoặc 15. Ở đây, bất cứ điều gì ngoài hành vi tuyến tính hoặc gần tuyến tính đều đủ. 

Điểm tinh tế chính là một ý tưởng tham lam có thể có vẻ hợp lý nhưng vẫn cần sự biện minh: khi một nhóm lớn hơn nhiều so với nhóm kia, việc buộc luân phiên là không thể vượt quá một điểm. Cấu trúc xen kẽ ngây thơ như “BGBGBG…” nghiêm ngặt sẽ bị hỏng khi một bên hết và các cách khác nhau để hoàn thiện các ký tự còn sót lại có thể ảnh hưởng đến tính chính xác nếu thực hiện không nhất quán. Một sai lầm tiềm ẩn khác là bắt đầu với giới tính sai khi số lượng có sự khác biệt đáng kể, điều này có thể làm giảm số lượng các vùng lân cận xen kẽ. 

Ví dụ: nếu chúng ta có 1 bé trai và 5 bé gái, một nỗ lực xen kẽ bất cẩn như bắt đầu bằng bé trai sẽ tạo ra “BGBGGG”, điều này nhanh chóng khiến các bé trai kiệt sức và sau đó sụp đổ thành một khối dài các bé gái. Bắt đầu với nhóm lớn hơn “GBGGGG” đạt được số lượng luân phiên tương đương hoặc tốt hơn và tránh được sự thất bại sớm không cần thiết của luân phiên. 

Vì vậy, nhiệm vụ thực sự là quyết định cách xen kẽ hai tập hợp để tối đa hóa các thay đổi lân cận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi hoán vị có thể có của nhiều tập hợp chứa n bản sao của một ký tự và m của ký tự kia, tính toán số lượng không khớp liền kề cho mỗi ký tự và giữ lại giá trị tốt nhất. Điều này đúng vì nó đánh giá trực tiếp tất cả các cấu hình. Tuy nhiên, số hoán vị là$\frac{(n+m)!}{n!m!}$, vốn đã rất lớn ngay cả đối với đầu vào vừa phải. Với n = m = 100, giá trị này lớn về mặt thiên văn, khiến cho việc liệt kê hoàn toàn không khả thi. 

Quan sát quan trọng là cách duy nhất để tạo ra sự không khớp giữa các vị trí liền kề là đặt các ký tự khác nhau cạnh nhau. Để tối đa hóa điều này, chúng tôi muốn thay thế miễn là cả hai loại đều có sẵn. Do đó, cấu trúc tốt nhất có thể là một chuỗi luân phiên cho đến khi hết một loại, sau đó phần còn lại phải tạo thành một khối duy nhất. Quyết định duy nhất còn lại là chúng ta bắt đầu với ký tự thường xuyên hơn hay ít thường xuyên hơn. 

Bắt đầu với ký tự đa số là tối ưu vì nó trì hoãn việc cạn kiệt ký tự đó, vốn là nguồn tài nguyên hạn chế. Nếu chúng ta bắt đầu với ký tự thiểu số, chúng ta có thể dùng hết ký tự đó sớm hơn theo cách tạo ra sự liền kề của ký tự giống nhau không cần thiết ở ranh giới. 

Vì vậy, cấu trúc tối ưu là tham lam: luôn đặt ký tự còn lại hiện tại thường xuyên hơn nếu nó giúp duy trì sự luân phiên, nhưng trên thực tế, điều này đơn giản hóa việc bắt đầu với số lượng lớn hơn và xen kẽ cho đến khi hết một bên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+m)!) | O(n+m) | Quá chậm | 
| Tham lam tối ưu | O(n+m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng chuỗi từng bước một, luôn cố gắng giữ các ký tự liền kề khác nhau càng lâu càng tốt. 

1. Xác định nhóm nào lớn hơn ngay từ đầu. Chúng tôi so sánh số lượng nam và nữ, vì nhóm lớn hơn thường chiếm ưu thế ở vị trí xuất phát. Lựa chọn này ngăn chặn sự kiệt sức sớm của nhóm nhỏ hơn khỏi việc phá vỡ luân phiên sớm. 
2. Khởi tạo hai bộ đếm đại diện cho bé trai và bé gái còn lại. Chúng tôi cũng quyết định ký tự bắt đầu là ký tự có số lượng cao hơn. Sự lựa chọn này tối đa hóa thời gian chúng ta có thể luân phiên mà không bị buộc phải lặp lại. 
3. Liên tục nối các ký tự trong khi cả hai số đều dương. Ở mỗi bước, chúng tôi nối thêm ký tự khác với ký tự được thêm cuối cùng nhưng chỉ khi ký tự đó vẫn còn hạn ngạch. Nếu không có lựa chọn xen kẽ ưa thích, chúng tôi buộc phải sử dụng loại còn lại. 
4. Khi một trong các số đếm trở thành 0, hãy nối thêm tất cả các ký tự còn lại của loại kia. Ở giai đoạn này, không thể thay thế thêm được nữa, vì vậy chúng tôi chấp nhận một khối ký tự giống hệt nhau. 

Việc xây dựng đảm bảo rằng chúng tôi luân phiên bất cứ khi nào có thể, điều này tương ứng trực tiếp với việc tạo ra sự không khớp giữa các vị trí liền kề. 

### Tại sao nó hoạt động 

Ở mỗi bước có sẵn cả hai loại, việc đặt hai ký tự giống hệt nhau cạnh nhau không bao giờ có thể cải thiện câu trả lời vì nó tiêu tốn tính linh hoạt mà không làm tăng cơ hội thay thế trong tương lai. Miễn là cả hai bộ đếm đều dương thì luôn tồn tại một lựa chọn giữ cho các giá trị kề xen kẽ nhau. Khi một bộ đếm chạm 0, về mặt cấu trúc không thể thay thế được nữa, do đó phân đoạn còn lại không thể được cải thiện bằng cách sắp xếp lại. Điều này làm cho quyết định tham lam trở nên tối ưu cục bộ ở mỗi bước và tối ưu toàn cục cho toàn bộ chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    # 'B' for boys, 'G' for girls
    if n >= m:
        b, g = n, m
        first, second = 'B', 'G'
    else:
        b, g = m, n
        first, second = 'G', 'B'
    
    res = []
    
    turn = first  # start with majority
    
    while b > 0 or g > 0:
        if turn == first:
            if b > 0:
                res.append(first)
                b -= 1
            else:
                res.append(second)
                g -= 1
        else:
            if g > 0:
                res.append(second)
                g -= 1
            else:
                res.append(first)
                b -= 1
        
        # try to alternate next
        if turn == first:
            turn = second
        else:
            turn = first
    
    print("".join(res))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp quá trình tham lam. Trước tiên, chúng tôi chuẩn hóa ký tự nào được coi là “đa số” để đơn giản hóa logic. Biến`turn`đại diện cho những gì chúng ta muốn đặt bên cạnh để duy trì sự luân phiên. Nếu ký tự đó không có thì chúng ta sẽ tự động quay lại ký tự kia. 

Một điểm tinh tế là chúng ta không bao giờ đếm rõ ràng các cặp xen kẽ. Thay vào đó, công trình đảm bảo sự xen kẽ tối đa về mặt cấu trúc, do đó không cần tính điểm. Vòng lặp chạy chính xác n + m lần lặp, do đó không có sự kém hiệu quả tiềm ẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`3 3`Chúng ta coi mỗi bên là đa số vì số lượng bằng nhau, giả sử chúng ta bắt đầu bằng`B`. 

| Bước | Còn lại B | Còn G | Xoay | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | B | B | 
| 2 | 2 | 2 | G | BG | 
| 3 | 1 | 2 | B | BGB | 
| 4 | 1 | 1 | G | BGBG | 
| 5 | 0 | 1 | B | BGBGB | 
| 6 | 0 | 0 | G | BGBGBG | 

Dấu vết này cho thấy sự luân phiên được duy trì đầy đủ cho đến khi cả hai bên đều kiệt sức, đó là kịch bản lý tưởng. 

### Ví dụ 2 

đầu vào:`1 4`Chúng tôi coi con gái là đa số, vì vậy hãy bắt đầu với G. 

| Bước | Còn lại B | Còn G | Xoay | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | G | G | 
| 2 | 1 | 2 | B | GB | 
| 3 | 0 | 2 | G | GBG | 
| 4 | 0 | 1 | B | GBGG | 
| 5 | 0 | 0 | G | GBGGG | 

Ở đây sự luân phiên chỉ tiếp tục cho đến khi cậu bé độc thân bị tiêu diệt. Sau đó, chỉ còn lại các cô gái, tạo thành khối cuối cùng. Bất kỳ thứ tự nào khác sẽ tạo ra cùng một hậu tố không thể thay thế không thể tránh khỏi, nhưng việc bắt đầu với đa số sẽ giảm thiểu sự gián đoạn trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi ký tự được thêm vào đúng một lần | 
| Không gian | O(n + m) | Lưu trữ chuỗi đầu ra | 

Các giới hạn n, m ≤ 100 khiến điều này trở nên tầm thường về mặt hiệu suất. Ngay cả khi các giới hạn lớn hơn nhiều, thì cách xây dựng tuyến tính tương tự vẫn sẽ là tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    n, m = map(int, input().split())
    
    if n >= m:
        b, g = n, m
        first, second = 'B', 'G'
    else:
        b, g = m, n
        first, second = 'G', 'B'
    
    res = []
    turn = first
    
    while b > 0 or g > 0:
        if turn == first:
            if b > 0:
                res.append(first)
                b -= 1
            else:
                res.append(second)
                g -= 1
        else:
            if g > 0:
                res.append(second)
                g -= 1
            else:
                res.append(first)
                b -= 1
        
        turn = second if turn == first else first
    
    print("".join(res))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    
    solve()
    
    out = sys.stdout.getvalue().strip()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# provided samples
assert run("3 3") in ["GBGBGB", "BGBGBG"], "sample 1"

# custom cases
assert run("1 1") in ["BG", "GB"], "minimum balanced"
assert run("1 4") == "GBGGG", "unbalanced small"
assert run("4 1") == "BGBBB", "symmetric unbalanced"
assert run("2 2") in ["BGBG", "GBGB"], "even split"
assert len(run("100 100")) == 200, "max size length check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | BG hoặc GB | cấu trúc xen kẽ tối thiểu | 
| 1 4 | GBGGG | xử lý tràn đa số | 
| 4 1 | BGBBB | mất cân bằng đối xứng | 
| 2 2 | BGBG hoặc GBGB | sự thay thế hoàn hảo | 
| 100 100 | chiều dài 200 chuỗi | hiệu suất và giới hạn | 

## Vỏ cạnh 

Khi một bên chỉ khác nhau đúng một, chẳng hạn như`2 1`, thuật toán sẽ luân phiên càng lâu càng tốt rồi nối thêm ký tự còn lại. Vì`2 1`, bắt đầu từ phía lớn hơn mang lại một chuỗi như`BGB`, đã đạt được hai ranh giới xen kẽ tối đa có thể. 

Khi một bên lớn hơn nhiều, chẳng hạn như`1 5`, bước đầu tiên tạo ra một luân phiên duy nhất và mọi thứ sau đó sẽ trở thành một khối. Thuật toán đảm bảo chính xác rằng yếu tố thiểu số duy nhất không bị lãng phí ở giữa theo cách có thể làm giảm cơ hội thay thế sớm. 

Khi số lượng bằng nhau, lựa chọn bắt đầu là hợp lệ. Việc triển khai có thể tạo ra mô hình xen kẽ và cả hai đều đạt được số lần chuyển đổi tối đa có thể vì không có sự mất cân bằng sẽ sớm sụp đổ.
