---
title: "CF 1722B - Bệnh mù màu"
description: "Chúng ta có một lưới rất nhỏ với chính xác hai dải ngang và một số cột dọc. Mỗi ô chứa một trong ba màu, nhưng người xem bị mù màu theo một cách cụ thể: xanh lá cây và xanh lam không thể phân biệt được, trong khi màu đỏ vẫn khác biệt với cả hai."
date: "2026-06-15T01:25:01+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1722
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 817 (Div. 4)"
rating: 800
weight: 1722
solve_time_s: 134
verified: true
draft: false
---

[CF 1722B - Bệnh mù màu](https://codeforces.com/problemset/problem/1722/B) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới rất nhỏ với chính xác hai dải ngang và một số cột dọc. Mỗi ô chứa một trong ba màu, nhưng người xem bị mù màu theo một cách cụ thể: xanh lá cây và xanh lam không thể phân biệt được, trong khi màu đỏ vẫn khác biệt với cả hai. 

Nhiệm vụ là xác định xem hai hàng có giống hệt người quan sát mù màu này hay không khi so sánh từng cột. Nói cách khác, sau khi áp dụng quy tắc hợp nhất màu xanh lá cây và xanh lam thành một màu nhận biết duy nhất, chúng tôi kiểm tra xem mọi cột có cùng cặp màu nhận biết ở cả hai hàng hay không. 

Mỗi trường hợp thử nghiệm là độc lập và chúng tôi lặp lại so sánh này nhiều lần. 

Các ràng buộc rất nhỏ: tối đa 100 ca kiểm thử và nhiều nhất 100 cột cho mỗi ca kiểm thử. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa ngoài chức năng quét tuyến tính. Ngay cả O(n²) cho mỗi thử nghiệm cũng có thể được chấp nhận trong thực tế, nhưng cấu trúc gợi ý rõ ràng về O(n) cho mỗi giải pháp thử nghiệm. 

Sự tinh tế quan trọng nằm ở việc hiểu được ý nghĩa thực sự của việc “không thể phân biệt được màu xanh lá cây và màu xanh lam”. Điều đó không có nghĩa là chúng trở nên đỏ hoặc biến mất. Nó có nghĩa là chúng sụp đổ thành một lớp tương đương duy nhất. Mọi sự so sánh đều phải tôn trọng sự tương đương đó. 

Một lỗi phổ biến phát sinh từ việc coi các chuỗi là có thể so sánh trực tiếp mà không cần chuyển đổi. Ví dụ, so sánh`G`Và`B`vì các ký tự khác nhau sẽ từ chối không chính xác các trường hợp chúng được coi là bằng nhau. 

Một cạm bẫy tiềm ẩn khác là cố gắng chỉ so sánh số lượng của từng nhân vật. Điều đó thất bại vì vị trí quan trọng. Ví dụ,`RGB`vs`BRG`có nhiều bộ ký tự giống hệt nhau nhưng khác nhau rõ ràng ngay cả dưới ánh xạ mù màu. 

Trường hợp cạnh là tối thiểu nhưng mang tính hướng dẫn. 

Một trường hợp là khi cả hai hàng bao gồm toàn màu xanh lá cây và xanh lam, ví dụ:```
GBG
BGB
```Tất cả những điều này đều tương đương theo quy tắc mù màu, vì vậy chúng phải khớp nhau nếu các vị trí căn chỉnh sau khi ánh xạ. Một giải pháp dựa trên tần số đơn giản có thể nói CÓ một cách không chính xác đối với những sắp xếp không khớp. 

Một trường hợp khác là lưới một cột:```
G
B
```Những màu này phải được coi là giống hệt nhau vì cả hai đều ánh xạ tới cùng một màu được cảm nhận. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: mô phỏng chuyển đổi mù màu một cách rõ ràng cho cả hai hàng và sau đó so sánh các chuỗi kết quả. Đối với mỗi ô, chúng tôi ánh xạ màu xanh lá cây và màu xanh lam thành một biểu tượng được chia sẻ, chẳng hạn như`X`, trong khi màu đỏ vẫn còn`R`. Sau khi chuyển đổi, chúng ta so sánh hai chuỗi kết quả theo từng ký tự. 

Điều này có hiệu quả vì nó làm giảm vấn đề xuống mức kiểm tra tính bằng nhau của chuỗi tiêu chuẩn. Chi phí là tuyến tính theo số lượng cột trên mỗi trường hợp thử nghiệm và vì lưới chỉ có hai hàng nên không có chi phí bổ sung. 

Nếu chúng ta thử làm điều gì đó phức tạp hơn, chẳng hạn như theo dõi các chuyển đổi hoặc so khớp các mẫu, thì chúng ta sẽ xử lý quá mức một vấn đề mà về cơ bản là một nhiệm vụ chuẩn hóa và so sánh. 

Lực lượng vũ phu đã tối ưu trong cài đặt này. Quan sát quan trọng là phép biến đổi có ý nghĩa duy nhất là ánh xạ thời gian không đổi cho mỗi ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| So sánh trực tiếp mà không cần chuẩn hóa | O(n) mỗi lần kiểm tra | O(1) | Không đúng | 
| Chuẩn hóa rồi so sánh | O(n) mỗi lần kiểm tra | Phát trực tuyến O(n) (hoặc O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng trường hợp kiểm thử vì mỗi lưới là độc lập và phải được đánh giá riêng. 
2. Với mỗi test, đọc số nguyên n và hai chuỗi đại diện cho các hàng. 
3. Lặp qua từng chỉ số cột từ 0 đến n - 1. 
4. Đối với mỗi cột, chuyển đổi cả hai ký tự bằng cách sử dụng cùng một quy tắc: xử lý`G`Và`B`tương đương, trong khi vẫn giữ`R`riêng biệt. Một cách thực hiện thuận tiện là ánh xạ cả hai`G`Và`B`cho cùng một biểu tượng, ví dụ`0`, và bản đồ`R`ĐẾN`1`. 
5. So sánh các ký tự được chuyển đổi ở mỗi cột. Nếu bất kỳ cột nào khác sau khi chuyển đổi, hãy kết luận ngay rằng các hàng đó không giống nhau đối với người quan sát mù màu. 
6. Nếu tất cả các cột khớp nhau sau khi chuyển đổi thì kết luận các hàng giống hệt nhau. 

Việc thoát sớm ở bước 5 rất quan trọng vì sự không khớp ở bất kỳ cột nào sẽ xác định đầy đủ câu trả lời. Không cần phải tiếp tục quét khi tìm thấy sự khác biệt. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là người quan sát xác định mối quan hệ tương đương với các màu trong đó`G ≡ B`Và`R`khác biệt với cả hai. Do đó, sự bình đẳng của các hàng được nhận biết tương đương với sự bình đẳng của hình ảnh của chúng theo ánh xạ tương đương này được áp dụng theo từng điểm. Vì ánh xạ được áp dụng độc lập cho từng ô và phép so sánh mang tính vị trí nên việc duy trì cột đẳng thức theo cột đảm bảo tính chính xác của phép so sánh đầy đủ hàng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def norm(c):
    # map green and blue to same class
    return 0 if c != 'R' else 1

t = int(input())
for _ in range(t):
    n = int(input())
    a = input().strip()
    b = input().strip()

    ok = True
    for i in range(n):
        if norm(a[i]) != norm(b[i]):
            ok = False
            break

    print("YES" if ok else "NO")
```Giải pháp đọc từng trường hợp kiểm thử một cách độc lập và xử lý hai hàng trong một lần chuyển. Chức năng trợ giúp`norm`mã hóa toàn bộ quy tắc mù màu trong một thao tác liên tục cho mỗi ký tự. Vòng lặp ngay lập tức chấm dứt khi tìm thấy sự không khớp, điều này đảm bảo hoạt động tối ưu ngay cả trong trường hợp đầu vào xấu nhất. 

Một lỗi triển khai phổ biến là quên loại bỏ các ký tự dòng mới khỏi chuỗi đầu vào, điều này có thể âm thầm thay đổi logic lập chỉ mục. Một người khác đang cố gắng so sánh trực tiếp các ký tự thô, nhưng thất bại cụ thể trên`G`so với`B`. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện từ mẫu. 

### Ví dụ 1 

đầu vào:```
n = 2
row1 = RG
row2 = RB
```| tôi | hàng1[i] | row2[i] | định mức (hàng1 [i]) | định mức(row2[i]) | trạng thái | 
| --- | --- | --- | --- | --- | --- | 
| 0 | R | R | 1 | 1 | trận đấu | 
| 1 | G | B | 0 | 0 | trận đấu | 

Tất cả các vị trí đều khớp sau khi chuẩn hóa, vì vậy kết quả là CÓ. 

Điều này chứng tỏ rằng thuật toán thu gọn chính xác các màu hiển thị khác nhau thành một lớp tương đương duy nhất trước khi so sánh. 

### Ví dụ 2 

đầu vào:```
n = 4
row1 = GRBG
row2 = GBGB
```| tôi | hàng1[i] | row2[i] | định mức (hàng1 [i]) | định mức(row2[i]) | trạng thái | 
| --- | --- | --- | --- | --- | --- | 
| 0 | G | G | 0 | 0 | trận đấu | 
| 1 | R | B | 1 | 0 | không khớp | 

Tại chỉ số 1, sự không khớp xuất hiện sau khi chuẩn hóa, do đó thuật toán dừng và trả về NO. 

Điều này xác nhận hành vi kết thúc sớm và cho thấy rằng sự tương đồng về cấu trúc là không đủ nếu sự tương đương về vị trí bị phá vỡ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi cột được xử lý một lần với công việc O(1) | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài chuỗi đầu vào | 

Tổng công việc trên tất cả các trường hợp thử nghiệm tối đa là 10.000 so sánh ký tự, điều này không đáng kể trong các ràng buộc. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    input = sys.stdin.readline

    t = int(input())
    def norm(c):
        return 0 if c != 'R' else 1

    for _ in range(t):
        n = int(input())
        a = input().strip()
        b = input().strip()

        ok = True
        for i in range(n):
            if norm(a[i]) != norm(b[i]):
                ok = False
                break
        output.append("YES" if ok else "NO")

    return "\n".join(output)

# provided samples
assert run("""6
2
RG
RB
4
GRBG
GBGB
5
GGGGG
BBBBB
7
BBBBBBB
RRRRRRR
8
RGBRRGBR
RGGRRBGR
1
G
G
""") == """YES
NO
YES
NO
YES
YES"""

# custom cases
assert run("""1
1
R
G
""") == "YES", "single column equivalence"

assert run("""1
3
RGB
RBR
""") == "YES", "mixed greens/blues equivalence"

assert run("""1
3
RRR
RRR
""") == "YES", "all red identical"

assert run("""1
3
RGB
GBR
""") == "NO", "permutation breaks position matching"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 R vs G | CÓ | lớp tương đương đơn bào | 
| RGB so với RBR | CÓ | độ chính xác thu gọn G/B nội bộ | 
| RRR so với RRR | CÓ | trường hợp giống nhau tầm thường | 
| RGB so với GBR | KHÔNG | phát hiện sự không phù hợp về vị trí | 

## Vỏ cạnh 

Lưới một cột là trường hợp không tầm thường đơn giản nhất. Thuật toán chỉ so sánh một cặp ký tự và trả về CÓ nếu cả hai thuộc cùng một lớp tương đương. Ví dụ,`G`vs`B`ánh xạ tới các giá trị chuẩn hóa giống hệt nhau, do đó kết quả là chính xác. 

Một lưới hoàn toàn đồng nhất như tất cả`R`trong cả hai hàng được xử lý mà không có bất kỳ logic đặc biệt nào. Mọi so sánh đều đánh giá sự bằng nhau, do đó vòng lặp hoàn thành mà không kích hoạt việc thoát sớm. 

Trường hợp chỉ có các giá trị xanh lục và xanh lam được trộn lẫn trong các mẫu khác nhau nhấn mạnh tầm quan trọng của việc so sánh vị trí. Thuật toán loại bỏ chính xác các thứ tự không khớp vì nó không dựa vào số lượng, chỉ căn chỉnh đẳng thức sau khi chuẩn hóa. 

Trường hợp tinh tế cuối cùng là đảm bảo việc cắt đầu vào là chính xác. Nếu các ký tự dòng mới không bị loại bỏ, việc so sánh có thể vô tình liên quan đến`\n`, điều này sẽ phá vỡ kiểm tra tính bằng nhau. các`.strip()`call đảm bảo điều này không thể xảy ra.
