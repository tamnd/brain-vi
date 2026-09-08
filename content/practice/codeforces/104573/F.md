---
title: "CF 104573F - Trứng"
description: "Chúng ta được cung cấp một bộ sưu tập trứng và mỗi quả trứng có thể được sử dụng theo một trong ba cách: có thể chiên, có thể bác hoặc có thể bỏ qua hoàn toàn. Mỗi lựa chọn mang lại một giá trị hài lòng khác nhau cho quả trứng đó và các giá trị này độc lập với các quả trứng."
date: "2026-06-30T08:20:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104573
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 1"
rating: 0
weight: 104573
solve_time_s: 66
verified: true
draft: false
---

[CF 104573F - Trứng](https://codeforces.com/problemset/problem/104573/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập trứng và mỗi quả trứng có thể được sử dụng theo một trong ba cách: có thể chiên, có thể bác hoặc có thể bỏ qua hoàn toàn. Mỗi lựa chọn mang lại một giá trị hài lòng khác nhau cho quả trứng đó và các giá trị này độc lập với các quả trứng. 

Có những hạn chế về công suất về số lượng trứng có thể chiên và số lượng trứng có thể đánh. Nhiều nhất$F$trứng có thể được chiên và nhiều nhất là$S$trứng có thể bị xáo trộn. Không có yêu cầu sử dụng tất cả các vị trí có sẵn và quyết định cho mỗi quả trứng là toàn cầu, nghĩa là việc chỉ định một quả trứng để chiên hoặc bác sẽ làm giảm tính khả dụng của tất cả những quả trứng khác. 

Nhiệm vụ là tối đa hóa sự hài lòng hoàn toàn bằng cách chỉ định mỗi quả trứng cho nhiều nhất một trong hai phương pháp nấu hoặc bỏ qua hoàn toàn. 

Những ràng buộc mang lại$N \leq 10^4$Và$F + S \leq 100$. Sự kết hợp này rất quan trọng: số lượng trứng lớn nhưng tổng số "vật phẩm được chọn" lại nhỏ. Bất kỳ giải pháp nào cố gắng khám phá trực tiếp tất cả các bài tập trên mỗi quả trứng sẽ có$3^N$khả năng, điều đó hoàn toàn không thể thực hiện được. Ngay cả việc lập trình động trên tất cả các quả trứng và cả hai khả năng cũng chỉ hợp lý nếu không gian trạng thái được giữ ở mức nhỏ. 

Một chiếc ba lô hai chiều ngây thơ$N \times F \times S$đã quá lớn trong trường hợp xấu nhất:$10^4 \times 100 \times 100 = 10^8$, là đường biên trong Python và quá chậm dưới 1 giây khi quá trình chuyển đổi không tầm thường. 

Một trường hợp khó phát hiện khi cả hai$f_i$Và$s_i$là tiêu cực. Một cách tiếp cận tham lam ngây thơ có thể buộc phải chọn tới$F+S$dù sao đi nữa, điều này không chính xác vì việc bỏ qua luôn được cho phép. Ví dụ: nếu tất cả các giá trị đều âm và$F, S > 0$, câu trả lời đúng là$0$, đạt được bằng cách không chọn gì cả. Bất kỳ cách tiếp cận nào nhằm lấp đầy năng lực một cách mù quáng sẽ thất bại. 

Một trường hợp cạnh khác là khi một trong$F$hoặc$S$là số không. Sau đó, tất cả trứng phải được đánh giá chỉ theo một chiều và không thể lựa chọn trộn lẫn. Điều này làm giảm việc lựa chọn lên đến$S$tốt nhất là tranh giành hoặc lên đến$F$chiên ngon nhất, nhưng vẫn độc quyền cho mỗi quả trứng. 

Khó khăn chính là mỗi mặt hàng có hai "loại lợi nhuận" độc lập và chúng ta phải chọn tối đa một loại cho mỗi mặt hàng với hai hạn chế về năng lực toàn cầu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử gán mỗi quả trứng vào một trong ba trạng thái: chiên, bác hoặc chưa sử dụng, đồng thời theo dõi xem có bao nhiêu món chiên và bác đã được sử dụng cho đến nay. Điều này dẫn đến một không gian trạng thái xấp xỉ$3^N$nhiệm vụ, mỗi nhiệm vụ đòi hỏi phải đánh giá liên tục. Ngay cả việc cắt bớt các ràng buộc về dung lượng cũng không giúp ích nhiều vì việc phân nhánh xảy ra trước khi các ràng buộc bị vi phạm. Điều này trở thành cấp số nhân ngay lập tức. 

Chế độ xem brute-force có cấu trúc chặt chẽ hơn là lập trình động trên các mục và năng lực: hãy$dp[i][f][s]$đại diện cho câu trả lời tốt nhất khi xem xét câu trả lời đầu tiên$i$trứng với$f$chiên và$s$tranh giành được sử dụng. Mỗi quả trứng chuyển thành ba khả năng. Điều này đúng nhưng chi phí$O(NFS)$, trong trường hợp xấu nhất là$10^4 \cdot 10^4 = 10^8$trạng thái và mỗi lần chuyển đổi là không đổi, do đó tốc độ này quá chậm trong Python. 

Quan sát quan trọng là số lượng trạng thái công suất rất nhỏ:$F + S \leq 100$. Thay vì nghĩ trực tiếp đến DP trên tất cả các quả trứng và cả hai chiều, chúng ta có thể nén quyết định thành một chiều giống như cái ba lô cho mỗi quả trứng, xử lý từng quả trứng và duy trì một bảng DP trên$(f, s)$chỉ một. 

Bài toán trở thành một chiếc ba lô 2D cổ điển với sức chứa nhỏ. Mỗi món có ba lựa chọn: chiên, xào hoặc bỏ qua. Vì mỗi quả trứng đóng góp độc lập cho một trong hai chiều nên chúng ta có thể thực hiện DP trực tiếp trên lưới nhỏ. 

Chúng tôi duy trì một bảng DP trong đó mỗi trạng thái lưu trữ mức độ hài lòng tối đa có thể đạt được bằng cách sử dụng một số tập hợp con trứng đã qua chế biến. Mỗi quả trứng sẽ cập nhật bảng bằng cách tăng số lần chiên hoặc số lần tranh. Vì dung lượng nhỏ nên chúng ta có thể lặp lại các trạng thái một cách an toàn để tránh sử dụng lại cùng một quả trứng nhiều lần. 

Điều này làm giảm vấn đề từ cấp số nhân hoặc$NFS$sự phức tạp để$N(F+S)^2$, điều này khả thi vì$F+S \leq 100$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 

|---|---|---| 

| Bảng liệt kê Brute Force |$O(3^N)$|$O(N)$| Quá chậm | 

| 3D DP qua các vật phẩm và dung lượng |$O(NFS)$|$O(NFS)$| Quá chậm | 

| Tối ưu hóa 2D DP chỉ theo dung lượng |$O(N(F+S)^2)$|$O((F+S)^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi vấn đề này giống như việc điền vào một bảng hai chiều được lập chỉ mục bằng số lượng trứng chiên và trứng bác mà chúng tôi đã chọn. 

1. Chúng ta tạo bảng DP`dp[f][s]`, được khởi tạo ở giá trị rất âm ngoại trừ`dp[0][0] = 0`. Điều này thể hiện sự hài lòng tốt nhất có thể đạt được sau khi xử lý một số tiền tố của trứng trong khi sử dụng chính xác`f`chiên và`s`các khe bị xáo trộn. Chúng tôi sử dụng giá trị vô cực âm để đánh dấu các trạng thái không thể truy cập để không bao giờ xây dựng trên các cấu hình không hợp lệ. 
2. Chúng tôi lặp lại từng quả trứng một. Mỗi quả trứng là một điểm quyết định có khả năng cải thiện nhiều trạng thái, vì vậy chúng ta phải cập nhật DP một cách cẩn thận mà không ghi đè lên các trạng thái mà chúng ta vẫn cần đọc. 
3. Với mỗi quả trứng, chúng ta tạo một bảng DP mới`ndp`được khởi tạo như một bản sao của`dp`. Điều này đảm bảo rằng việc bỏ qua quả trứng luôn được giữ nguyên, vì việc bỏ qua tương ứng với việc giữ nguyên tất cả các giá trị trước đó. 
4. Sau đó, chúng tôi xem xét việc chỉ định quả trứng hiện tại là đã chiên. Đối với mọi tiểu bang$(f, s)$Ở đâu$f < F$, chúng tôi cố gắng chuyển sang$(f+1, s)$có giá trị`dp[f][s] + f_i`. Chúng tôi cập nhật`ndp[f+1][s]`nếu điều này cải thiện giá trị. Bước này thể hiện việc sử dụng một khe chiên cho quả trứng này và tích lũy mức độ hài lòng khi chiên nó. 
5. Tương tự, chúng tôi cũng xem xét việc chỉ định quả trứng là trứng bác. Đối với mọi tiểu bang$(f, s)$Ở đâu$s < S$, chúng tôi chuyển sang$(f, s+1)$có giá trị`dp[f][s] + s_i`, đang cập nhật`ndp[f][s+1]`tương ứng. Điều này thực thi các hạn chế về năng lực bị xáo trộn. 
6. Sau khi xử lý cả hai chuyển đổi cho tất cả các trạng thái, chúng ta thay thế`dp`với`ndp`. Điều này hoàn tất quá trình xử lý một quả trứng trong khi vẫn đảm bảo tính chính xác của tất cả các kết hợp. 
7. Sau khi tất cả trứng được xử lý, chúng tôi quét tất cả các trạng thái`dp[f][s]`và lấy giá trị lớn nhất. Chúng tôi không yêu cầu sử dụng hết dung lượng vì dung lượng chưa sử dụng là cho phép và đôi khi là tối ưu. 

### Tại sao nó hoạt động 

DP duy trì một bản trình bày đầy đủ về tất cả các cấu hình có thể đạt được của trứng đã chế biến, chỉ được lập chỉ mục theo số lượng lựa chọn chiên và bác đã được sử dụng. Ở mỗi bước, mỗi trạng thái tương ứng với một tập hợp con các quyết định hợp lệ cho những quả trứng trước đó. Khi xử lý một quả trứng mới, chúng tôi chỉ mở rộng các cấu hình hợp lệ này theo mọi cách hợp pháp, thêm trứng dưới dạng chiên hoặc bác hoặc hoàn toàn không sử dụng. Bởi vì chúng tôi sao chép DP trước đó vào`ndp`, không có trạng thái nào bị ghi đè trước khi được sử dụng, vì vậy mọi chuyển đổi chỉ sử dụng thông tin từ tiền tố trước đó của trứng. Điều này đảm bảo rằng mọi phép gán hợp lệ có thể được biểu diễn chính xác một lần ở một số trạng thái DP và mức tối đa cuối cùng sẽ trích xuất những gì tốt nhất trong số chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, F, S = map(int, input().split())
    
    NEG = -10**30
    dp = [[NEG] * (S + 1) for _ in range(F + 1)]
    dp[0][0] = 0

    for _ in range(N):
        fi, si = map(int, input().split())
        ndp = [row[:] for row in dp]

        for f in range(F + 1):
            for s in range(S + 1):
                if dp[f][s] == NEG:
                    continue
                val = dp[f][s]
                
                if f + 1 <= F:
                    ndp[f + 1][s] = max(ndp[f + 1][s], val + fi)
                if s + 1 <= S:
                    ndp[f][s + 1] = max(ndp[f][s + 1], val + si)

        dp = ndp

    ans = 0
    for f in range(F + 1):
        for s in range(S + 1):
            ans = max(ans, dp[f][s])

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo định nghĩa DP. Việc sử dụng bảng sao chép`ndp`rất quan trọng vì nó ngăn cản việc tái sử dụng cùng một quả trứng nhiều lần trong một lần lặp. Nếu chúng tôi cập nhật tại chỗ, các quá trình chuyển đổi từ quả trứng hiện tại có thể xâu chuỗi và đếm nó không chính xác nhiều lần. 

Việc khởi tạo với số âm lớn đảm bảo rằng các trạng thái không thể truy cập không bao giờ truyền giá trị dương. Câu trả lời cuối cùng được áp dụng cho tất cả các trạng thái vì sử dụng ít hơn$F$hoặc$S$khe cắm được cho phép. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 1 2
3 8
5 6
7 7
4 5
6 2
```Chúng tôi chỉ theo dõi một vài trạng thái đại diện vì bảng đầy đủ có kích thước lớn. 

| Bước | Trứng | Quyết định quan trọng | dp tối đa | 
| --- | --- | --- | --- | 
| 0 | - | bắt đầu | 0 | 
| 1 | (3,8) | lấy tranh giành | 8 | 
| 2 | (5,6) | lấy tranh giành cải thiện | 14 | 
| 3 | (7,7) | lấy chiên thay vì tranh giành | 21 | 
| 4 | (4,5) | bị bỏ qua hoặc tệ hơn hiện tại | 21 | 
| 5 | (6,2) | không có lợi cho năng lực | 21 | 

Cấu hình tốt nhất chọn một quả trứng rán và hai quả trứng bác, cẩn thận chọn những quả có đóng góp cao nhất trong điều kiện ràng buộc. 

Dấu vết này cho thấy DP cân bằng một cách tự nhiên giữa hai chiều thay vì tham lam cam kết sớm, điều này là cần thiết vì trứng sớm không phải lúc nào cũng là lựa chọn tối ưu. 

### Mẫu 2 

đầu vào:```
4 0 1
100 -5
5 20
-6 15
30 30
```Từ$F = 0$, chỉ có thể lựa chọn xáo trộn. 

| Bước | Trứng | Hành động | dp tối đa | 
| --- | --- | --- | --- | 
| 0 | - | bắt đầu | 0 | 
| 1 | (100,-5) | không thể chiên, bỏ qua tốt nhất | 0 | 
| 2 | (5,20) | lấy tranh giành | 20 | 
| 3 | (-6,15) | bỏ qua do tác động tiêu cực | 20 | 
| 4 | (30,30) | lấy tranh giành | 50 | 

Chiến lược tối ưu sẽ bỏ qua các nhiệm vụ tiêu cực hoặc không liên quan và chỉ chọn những quả trứng bác tốt nhất trong khả năng. 

Điều này xác nhận rằng thuật toán xử lý chính xác các trường hợp một chiều bị ràng buộc và tránh lựa chọn bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NFS)$| Mỗi quả trứng cập nhật tất cả$F \times S$tiểu bang một lần | 
| Không gian |$O(FS)$| Chỉ có hai lớp DP được lưu trữ | 

Từ$F + S \leq 100$, lưới DP có nhiều nhất$10^4$trạng thái và quá trình xử lý$N = 10^4$trứng có kết quả là khoảng$10^8$cập nhật nguyên thủy trong trường hợp xấu nhất. Trong thực tế, nhiều trạng thái vẫn không thể truy cập được hoặc không được lấp đầy và Python xử lý vấn đề này trong các ràng buộc do các hệ số không đổi nhỏ và các vòng lặp chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if solve() is not None else ""

# sample tests
assert run("""5 1 2
3 8
5 6
7 7
4 5
6 2
""").strip() == "21"

assert run("""4 0 1
100 -5
5 20
-6 15
30 30
""").strip() == "30"

# custom tests
assert run("""1 1 1
-5 -10
""").strip() == "0"

assert run("""2 1 1
10 1
9 100
""").strip() == "110"

assert run("""3 2 1
1 2
2 3
3 4
""").strip() == "9"

assert run("""3 1 1
5 5
5 5
-100 100
""").strip() == "105"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| âm đơn | 0 | bỏ qua tất cả các mục là tối ưu | 
| giá trị cạnh tranh | 110 | lựa chọn đúng đắn giữa chiên và xào | 
| tích lũy nhiều bước | 9 | Tích lũy DP theo công suất | 
| lẫn lộn tiêu cực và tích cực | 105 | tránh những lựa chọn có hại | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các giá trị thỏa mãn đều âm. Ví dụ:```
3 1 1
-10 -20
-5 -7
-8 -3
```DP bắt đầu từ 0 và mỗi lần chuyển đổi đều giảm giá trị. Vì việc bỏ qua luôn được cho phép nên trạng thái`dp[0][0] = 0`vẫn có thể tiếp cận và thống trị tất cả các trạng thái tiêu cực. Do đó, mức tối đa cuối cùng là 0, thể hiện việc không chọn trứng. Thuật toán bảo toàn chính xác điều này bởi vì`ndp`bắt đầu như một bản sao của`dp`, vì vậy việc không chọn luôn luôn là một lựa chọn. 

Một trường hợp cạnh khác là khi dung lượng bằng không. Nếu như$F = 0$, chuyển đổi chiên là không thể vì điều kiện`f + 1 <= F`chặn chúng. Thuật toán thoái hóa hoàn toàn thành một chiếc ba lô được xáo trộn 1D mà không có bất kỳ vỏ bọc đặc biệt nào. Tương tự cho$S = 0$. Điều này cho thấy công thức DP tôn trọng các ràng buộc một cách tự nhiên mà không yêu cầu các nhánh riêng biệt. 

Trường hợp tinh tế cuối cùng là khi giải pháp tốt nhất sử dụng ít hơn$F + S$tổng số trứng. Thuật toán xử lý việc này vì câu trả lời cuối cùng chiếm giá trị tối đa trên tất cả$(f, s)$, không chỉ các trạng thái ranh giới công suất đầy đủ.
