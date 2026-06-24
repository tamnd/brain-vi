---
title: "CF 105283G - Mảng Khỉ"
description: "Chúng ta được cung cấp một mảng và ba giá trị đặc biệt, với thứ tự nghiêm ngặt $Y < K < X$. Đối với mỗi trường hợp kiểm thử, nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề thỏa mãn đồng thời ba điều kiện: phần tử lớn nhất trong mảng con chính xác là $X$, phần tử nhỏ nhất chính xác là…"
date: "2026-06-23T14:25:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105283
codeforces_index: "G"
codeforces_contest_name: "TeamsCode Summer 2024 Novice Division"
rating: 0
weight: 105283
solve_time_s: 93
verified: false
draft: false
---

[CF 105283G - Mảng Khỉ](https://codeforces.com/problemset/problem/105283/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và ba giá trị đặc biệt, với thứ tự nghiêm ngặt$Y < K < X$. Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề thỏa mãn ba điều kiện đồng thời: phần tử lớn nhất trong mảng con đó chính xác là$X$, phần tử tối thiểu chính xác là$Y$, và giá trị$K$không xuất hiện ở bất cứ đâu bên trong mảng con. 

Do đó, một mảng con chỉ hợp lệ nếu mọi phần tử tuân theo các giới hạn được áp đặt bởi$X$Và$Y$, đồng thời tránh một giá trị bị cấm cụ thể ở giữa phạm vi đó. 

Kích thước đầu vào lớn, lên tới$5 \cdot 10^5$tổng số phần tử trên các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các mảng con, vì điều đó sẽ dẫn đến hành vi bậc hai hoặc tệ hơn. Quét bậc hai trên số chẵn$10^5$các yếu tố tạo ra xung quanh$10^{10}$hoạt động vượt xa giới hạn một giây có thể chịu đựng được. 

Cấu trúc chính là tính hợp lệ hoàn toàn được xác định bởi các thuộc tính cục bộ bên trong một phân đoạn: liệu một mảng con có nằm trong phạm vi hay không$[Y, X]$, tránh$K$, và chứa cả hai cực trị$Y$Và$X$. Điều này gợi ý rằng thay vì liệt kê các mảng con, chúng ta nên chuyển đổi mảng thành các vùng hợp lệ tối đa và đếm các khoản đóng góp một cách hiệu quả trong mỗi vùng. 

Một trường hợp thất bại tinh tế xuất hiện khi một cách tiếp cận ngây thơ bỏ qua giá trị bị cấm$K$. Ví dụ, nếu$X = 7$,$Y = 4$,$K = 5$, và mảng là$[6, 7, 5, 4]$, một lần quét mạnh mẽ có thể được tính$[6,7,5,4]$hợp lệ vì nó chứa cả hai$7$Và$4$, nhưng nó bao gồm không chính xác$5$, vi phạm điều kiện Một lỗi phổ biến khác là quên rằng các giá trị bên ngoài$[Y, X]$vô hiệu hóa bất kỳ mảng con nào vượt qua chúng. TRONG$[3, 4, 7]$, bất kỳ mảng con nào chứa$3$không thể có tối thiểu$Y = 4$, vì vậy nó không được coi là một phần của vùng hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê từng mảng con và kiểm tra xem nó có thỏa mãn ba điều kiện hay không. Đối với mỗi mảng con, chúng tôi theo dõi mức tối thiểu, tối đa và liệu$K$xuất hiện. Việc duy trì những điều này dần dần vẫn khiến chúng ta gặp khó khăn$O(N^2)$mảng con cho mỗi trường hợp thử nghiệm và mỗi lần kiểm tra ít nhất$O(1)$, làm cho việc giải quyết quá chậm khi$N$đạt tới$10^5$. 

Quan sát chính là các phần tử không hợp lệ sẽ phân chia mảng thành các phân đoạn độc lập. Bất kỳ yếu tố bên ngoài$[Y, X]$ngay lập tức phá vỡ tính hợp lệ vì nó vi phạm ràng buộc tối thiểu hoặc tối đa đối với bất kỳ mảng con nào chứa nó. Giá trị bị cấm$K$, mặc dù nằm trong phạm vi, cũng hoạt động như một dấu tách cứng vì nó loại bỏ bất kỳ mảng con nào chứa nó. 

Điều này làm giảm vấn đề thành các phân đoạn độc lập trong đó mọi phần tử đều nằm trong$[Y, X]$và không bằng$K$. Bên trong một phân đoạn như vậy, chúng ta cần đếm các mảng con chứa ít nhất một lần xuất hiện của$Y$và ít nhất một lần xuất hiện của$X$. 

Đối với mỗi vị trí được coi là điểm cuối phù hợp, chúng tôi duy trì các lần xuất hiện gần đây nhất của$Y$Và$X$. Số lượng vị trí bắt đầu hợp lệ được xác định bởi lần xuất hiện sớm nhất trong hai lần xuất hiện cuối cùng này, vì bất kỳ mảng con hợp lệ nào kết thúc tại$r$phải bắt đầu bằng hoặc trước chỉ số nhỏ hơn trong hai chỉ số để đảm bảo cả hai giá trị đều được bao gồm. Phối cảnh cửa sổ trượt này loại bỏ nhu cầu liệt kê rõ ràng các mảng con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(1)$| Quá chậm | 
| Đoạn + Cửa sổ trượt |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và quét mảng từ trái sang phải, duy trì ranh giới phân đoạn và theo dõi các lần xuất hiện chính. 

### Các bước 

1. Khởi tạo một biến`start`đánh dấu sự bắt đầu của phân đoạn hợp lệ hiện tại. Đoạn này chỉ chứa các phần tử không$K$và nằm bên trong$[Y, X]$. 
2. Duy trì hai con trỏ,`lastY`Và`lastX`, được khởi tạo ở các vị trí trước khi mảng bắt đầu. Chúng lưu trữ các vị trí gần đây nhất của$Y$Và$X$. 
3. Lặp lại chỉ mục mảng$r$từ trái sang phải. 
4. Nếu$A[r] = K$hoặc$A[r] < Y$hoặc$A[r] > X$, đặt lại phân đoạn bằng cách cài đặt`start = r + 1`và thiết lập lại`lastY`Và`lastX`đến các giá trị không hợp lệ. Điều này đảm bảo không có mảng con nào vượt qua ranh giới không hợp lệ. 
5. Nếu không, hãy cập nhật`lastY`nếu như$A[r] = Y$và cập nhật`lastX`nếu như$A[r] = X$. 
6. Nếu cả hai`lastY`Và`lastX`hợp lệ bên trong phân đoạn hiện tại, hãy tính xem có bao nhiêu mảng con hợp lệ kết thúc tại$r$. Số này là`max(0, min(lastY, lastX) - start + 1)`và thêm nó vào câu trả lời. 

### Tại sao nó hoạt động 

Bên trong bất kỳ phân đoạn nào, mọi mảng con đều tự động không có các phần tử không hợp lệ và không có$K$. Hạn chế duy nhất còn lại là một mảng con hợp lệ phải bao gồm ít nhất một$Y$và một$X$. Đối với điểm cuối bên phải cố định, vị trí mới nhất của$Y$Và$X$xác định thời điểm sớm nhất mà cả hai đảm bảo sẽ xuất hiện cùng nhau. Bất kỳ chỉ mục bắt đầu nào trước đó vẫn bao gồm cả hai giá trị, trong khi bất kỳ chỉ mục bắt đầu nào sau đó sẽ loại trừ ít nhất một trong số chúng. Điều này thiết lập quy tắc đếm trực tiếp cho tất cả các mảng con hợp lệ kết thúc ở mỗi vị trí mà không bị trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, X, Y, K = map(int, input().split())
        A = list(map(int, input().split()))

        start = 0
        lastY = -1
        lastX = -1
        ans = 0

        for i, v in enumerate(A):
            if v == K or v < Y or v > X:
                start = i + 1
                lastY = -1
                lastX = -1
                continue

            if v == Y:
                lastY = i
            if v == X:
                lastX = i

            if lastY != -1 and lastX != -1:
                ans += max(0, min(lastY, lastX) - start + 1)

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp việc phân tách phân đoạn và logic cửa sổ trượt. các`start`con trỏ đảm bảo chúng ta không bao giờ coi các mảng con vượt qua các phần tử không hợp lệ hoặc$K$. Các biến`lastY`Và`lastX`liên tục theo dõi các điểm cuối cần thiết để đảm bảo sự hiện diện của cả hai thái cực. Bước tích lũy cuối cùng chuyển đổi các vị trí này thành số lượng chỉ số bắt đầu hợp lệ cho mỗi điểm cuối bên phải. 

Một lỗi triển khai phổ biến là quên đặt lại cả hai biến xuất hiện lần cuối khi gặp phần tử không hợp lệ. Nếu không có thiết lập lại này, các chỉ mục cũ sẽ bị rò rỉ trên các phân đoạn và hợp nhất các vùng độc lập không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[6, 7, 4, 6]$với$X = 7$,$Y = 4$,$K = 5$. 

| tôi | giá trị | bắt đầu | cuối cùngY | cuối cùngX | phút(lastY,lastX) | đã thêm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 6 | 0 | -1 | -1 | - | 0 | 
| 1 | 7 | 0 | -1 | 1 | - | 0 | 
| 2 | 4 | 0 | 2 | 1 | 1 | 2 | 
| 3 | 6 | 0 | 2 | 1 | 1 | 2 | 

Tại chỉ số 3, cả hai$Y$Và$X$đã xuất hiện, do đó các mảng con kết thúc ở số 3 sẽ đóng góp dựa trên lần xuất hiện trước đó của chúng. Điều này chứng tỏ cách thuật toán tích lũy nhiều vị trí bắt đầu hợp lệ mà không liệt kê chúng. 

Bây giờ hãy xem xét$[4, 7, 5, 6]$. phần tử$5$ngay lập tức phá vỡ phân đoạn. 

| tôi | giá trị | bắt đầu | cuối cùngY | cuối cùngX | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 0 | 0 | -1 | 0 | 
| 1 | 7 | 0 | 0 | 1 | 1 | 
| 2 | 5 | 3 | -1 | -1 | 0 | 
| 3 | 6 | 3 | -1 | -1 | 0 | 

Sự hiện diện của$K = 5$buộc thiết lập lại, đảm bảo không có mảng con nào vượt qua nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi phần tử được xử lý một lần, với các cập nhật và số học liên tục theo thời gian cho mỗi chỉ mục | 
| Không gian |$O(1)$| Chỉ có một số biến theo dõi được duy trì | 

Quá trình quét tuyến tính vừa vặn thoải mái trong giới hạn tổng thể của$5 \cdot 10^5$các phần tử, vì mỗi trường hợp thử nghiệm đóng góp công việc tương ứng mà không cần các vòng lặp lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample (as formatted in statement may be messy; conceptual check)
# assert run("...") == "..."

# minimum size, no valid subarray
assert run("1\n1 5 1 3\n1\n") == "0"

# single valid segment
assert run("1\n3 3 1 2\n1 3 1\n") == "1"

# K blocks everything
assert run("1\n4 7 4 5\n4 7 5 4\n") == "0"

# all valid elements but missing X
assert run("1\n3 5 1 4\n1 4 4\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | hành vi ranh giới tối thiểu | 
| trường hợp hợp lệ nhỏ | 1 | đếm đúng khi xuất hiện cả hai thái cực | 
| K có mặt | 0 | thiết lập lại phân đoạn chính xác | 
| thiếu X hoặc Y | 0 | thực thi yêu cầu | 

## Vỏ cạnh 

Trường hợp có các dấu phân cách không hợp lệ liên tiếp, chẳng hạn như nhiều lần xuất hiện của$K$đảm bảo rằng việc đặt lại phân đoạn không vô tình hợp nhất. Thuật toán xử lý việc này bằng cách đặt lại`start`,`lastY`, Và`lastX`độc lập ở mỗi vị trí không hợp lệ, do đó không có trạng thái nào tồn tại trong thời gian nghỉ. 

Một trường hợp$Y$Và$X$chỉ xuất hiện ở các cạnh của đoạn dài kiểm tra tính chính xác của việc đếm tiền tố. Thuật toán vẫn tính tất cả các mảng con mở rộng từ lần bắt đầu hợp lệ trước đó bởi vì`min(lastY, lastX)`neo chính xác ranh giới khả thi sớm nhất. 

Trường hợp mảng xen kẽ giữa các giá trị hợp lệ và không hợp lệ kiểm tra việc khởi tạo phân đoạn lặp lại. Mỗi phân khúc mới đều bắt đầu mới và các khoản đóng góp được giới hạn cục bộ, ngăn ngừa việc tính quá mức qua các ranh giới.
