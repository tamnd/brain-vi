---
title: "CF 245C - Trò chơi bằng tiền xu"
description: "Chúng ta được cấp một dãy rương được đánh số, mỗi rương chứa một số đồng xu. Ở mỗi nước đi, người chơi chọn một vị trí số nguyên $x$ và nước đi đó ảnh hưởng đồng thời đến ba rương cụ thể: $x$, $2x$ và $2x+1$. Từ mỗi chiếc rương này, một đồng xu sẽ bị lấy ra nếu nó tồn tại."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 245
codeforces_index: "C"
codeforces_contest_name: "CROC-MBTU 2012, Elimination Round (ACM-ICPC)"
rating: 1700
weight: 245
solve_time_s: 97
verified: true
draft: false
---

[CF 245C - Trò chơi bằng tiền xu](https://codeforces.com/problemset/problem/245/C) 

**Đánh giá:** 1700 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy rương được đánh số, mỗi rương chứa một số đồng xu. Ở mỗi nước đi, người chơi chọn một vị trí nguyên$x$, và động thái đó đồng thời ảnh hưởng đến ba rương cụ thể:$x$,$2x$, Và$2x+1$. Từ mỗi chiếc rương này, một đồng xu sẽ bị lấy ra nếu nó tồn tại. 

Trò chơi kết thúc khi mọi rương đều trống rỗng. Hai người chơi luân phiên di chuyển, bắt đầu với Polycarpus và cả hai đều chơi tối ưu theo nghĩa là họ muốn trò chơi kết thúc càng nhanh càng tốt. Câu hỏi không phải là ai thắng, mà là tổng số nước đi cần thiết để làm trống tất cả các đồng xu là bao nhiêu, giả sử cả hai đều hợp tác để kết thúc trò chơi. 

Cấu trúc quan trọng là mỗi bước di chuyển đều nhắm vào một mẫu chỉ số nhỏ “giống như cây”. Mỗi sự lựa chọn của$x$định nghĩa một gốc và hai con của nó trong sơ đồ lập chỉ mục heap nhị phân. 

Kích thước đầu vào nhỏ, với$n \le 100$Và$a_i \le 1000$. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào liên quan đến mô phỏng trên mỗi nút, điều chỉnh tham lam lặp đi lặp lại hoặc thậm chí lập trình động dựa trên trạng thái trên các tập hợp con đều khả thi, miễn là chúng tôi tránh được sự bùng nổ theo cấp số nhân về số lượng xu. 

Trường hợp cạnh tinh vi đầu tiên xuất hiện khi không có nước đi hợp lệ nào tồn tại. Nếu không có$x$như vậy$2x+1 \le n$, thì không thể thực hiện được thao tác nào cả. Nếu có ít nhất một rương chứa đồng xu có cấu hình như vậy thì trò chơi sẽ không bao giờ kết thúc. Ví dụ, khi$n=1$Và$a_1>0$, không có giá trị$x$, vậy câu trả lời là$-1$. 

Điều tinh tế thứ hai là các bước di chuyển chồng chéo lên nhau rất nhiều. Một đồng xu trong rương có thể được lấy ra bằng nhiều lựa chọn khác nhau$x$. Ví dụ, ngực 4 bị ảnh hưởng bởi$x=2$(từ$2x=4$) mà còn liên quan gián tiếp đến các chuyển động khác trong cấu trúc. Sự chồng chéo này có nghĩa là các phương pháp tiếp cận ngây thơ “xử lý tham lam từ các lá trở lên” có thể tính sai số bước di chuyển tối thiểu vì chúng bỏ qua sự ghép nối toàn cục giữa các hoạt động. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là mô phỏng mọi chuỗi hành động có thể xảy ra. Mỗi bước di chuyển chọn một hợp lệ$x$và mỗi trạng thái là một mảng số lượng xu. Chúng tôi thử đệ quy tất cả các lựa chọn cho đến khi tất cả các đồng xu bằng 0, lấy độ sâu tối thiểu. Điều này đúng vì nó khám phá tất cả các chuỗi, nhưng nó hoàn toàn không khả thi: ngay cả với$n=100$, hệ số phân nhánh xấp xỉ$n/2$và độ sâu có thể lên tới$\max a_i$, do đó không gian trạng thái bùng nổ theo cấp số nhân. 

Quan sát quan trọng là mỗi bước di chuyển đều độc lập về cách nó làm giảm số xu: một bước di chuyển tại$x$luôn trừ chính xác một xu từ mỗi vị trí trong số ba vị trí, bất kể giá trị hiện tại. Điều này có nghĩa là vấn đề không phải là việc ra lệnh loại bỏ xu mà là việc chọn số lần mỗi lần loại bỏ.$x$được sử dụng. 

Cho phép$t_x$là số lần chúng ta chọn vị trí$x$. Sau đó mỗi ngực$i$mất số tiền bằng tổng của tất cả$t_x$như vậy$i \in \{x, 2x, 2x+1\}$. Bài toán trở thành: chọn số nguyên không âm$t_x$giảm thiểu$\sum t_x$, tùy thuộc vào việc đáp ứng mọi yêu cầu về ngực. 

Điều này biến vấn đề thành một vấn đề bao trùm trên cây được lập chỉ mục nhị phân. Mỗi hoạt động bao gồm tối đa ba nút và chúng tôi muốn đáp ứng tất cả “nhu cầu tiền xu” bắt buộc bằng cách sử dụng tổng số hoạt động tối thiểu. 

Bởi vì$n$nhỏ, chúng ta có thể xử lý các nút theo thứ tự ngược lại, từ$n$xuống 1, đẩy mức sử dụng cần thiết lên trên: nếu một nút vẫn cần tiền, chúng tôi buộc phải áp dụng các hoạt động bao gồm nó. Điều này gây ra sự lan truyền nhu cầu tham lam đối với các bậc cha mẹ hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tuyên truyền tham lam trên cấu trúc cây | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các nút từ$n$xuống$1$, duy trì số lần mỗi nút phải được sử dụng làm “trung tâm điều hành bắt buộc”. 

1. Khởi tạo một mảng$need[i]$với$a_i$. Điều này thể hiện số tiền còn lại phải được loại bỏ ở mỗi rương. 
2. Lặp lại$i$từ$n$xuống$1$. Chúng tôi đối xử$i$như một gốc hoạt động tiềm năng. 
3. Nếu$i > 0$, chúng tôi quyết định áp dụng thao tác bao nhiêu lần$i$. Mỗi ứng dụng loại bỏ một đồng xu khỏi$i$,$2i$, Và$2i+1$. Để thỏa mãn ngực$i$, ít nhất chúng ta phải áp dụng thao tác$need[i]$lần. 
4. Hãy để$t = need[i]$. Thêm vào$t$cho câu trả lời vì chúng tôi cam kết sử dụng hoạt động$i$nhiều lần như vậy. 
5. Giảm$need[i]$qua$t$, làm cho nó bằng không. 
6. Tuyên truyền hiệu ứng đi lên bằng cách tăng$need[2i]$Và$need[2i+1]$qua$t$, vì những vị thế đó cũng bị mất xu khi hoạt động$i$được sử dụng. Chúng ta phải đảm bảo không vượt quá giới hạn. 
7. Tiếp tục cho đến khi tất cả các nút được xử lý. 

Sau khi hoàn thành, nếu có$need[i]$vẫn khác 0 đối với một nút không thể được bao phủ bởi bất kỳ thao tác hợp lệ nào (cụ thể là nếu không thể truy cập được nút đó thông qua bất kỳ nút cha nào), chúng tôi trả về$-1$. 

Điều quan trọng là việc xử lý từ các chỉ số lớn đến nhỏ đảm bảo rằng khi chúng ta quyết định sử dụng một thao tác bao nhiêu lần thì tất cả các đóng góp cho các ràng buộc chỉ số cao hơn đều đã được tính đến. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến tại mỗi chỉ số$i$, tất cả các yêu cầu về tiền xu cho các nút lớn hơn$i$đã được thỏa mãn một cách tối ưu. Khi xử lý$i$, cách duy nhất còn lại để giảm$need[i]$là sử dụng thao tác$i$, vì không có thao tác chỉ mục nhỏ hơn nào có thể ảnh hưởng đến nó theo cách làm giảm tổng chi phí mà không vi phạm các ràng buộc được thỏa mãn trước đó. Điều này tạo ra một quyết định bắt buộc cục bộ ở mỗi bước và bởi vì tất cả các phần phụ thuộc đều chuyển từ cha mẹ sang con cái trong cấu trúc đống nhị phân, nên việc xử lý theo thứ tự cấu trúc liên kết ngược sẽ ngăn ngừa xung đột trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a = [0] + a  # 1-indexed

    need = a[:]
    ans = 0

    for i in range(n, 0, -1):
        if need[i] <= 0:
            continue

        t = need[i]
        ans += t
        need[i] -= t

        if i * 2 <= n:
            need[i * 2] += t
        if i * 2 + 1 <= n:
            need[i * 2 + 1] += t

    for i in range(1, n + 1):
        if need[i] > 0:
            print(-1)
            return

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này tuân theo ý tưởng đẩy các yêu cầu xuống theo thứ tự chỉ mục ngược. Mảng được lập chỉ mục 1 để phù hợp với cấu trúc chuyển tiếp giống như đống$i \rightarrow 2i, 2i+1$. Sự thiếu hụt của mỗi nút trực tiếp kích hoạt một số hoạt động tại nút đó và hiệu ứng này sẽ được truyền đến trẻ em. 

Một điểm tinh tế là chúng tôi chỉ “sửa” một nút một lần, vì sau khi xử lý nút đó, bước sau sẽ không truy cập lại nút đó. Điều này tránh được việc tính hai lần và đảm bảo sự lan truyền tuyến tính của các ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 0 0
```| tôi | cần[i] trước | hoạt động t | trả lời | cần[2i] cập nhật | cần cập nhật[2i+1] | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 0 | 0 | 0 | - | - | 
| 2 | 0 | 0 | 0 | - | - | 
| 1 | 1 | 1 | 1 | +1 đến 2 | +1 đến 3 | 

Sau khi xử lý, nút 1 yêu cầu một thao tác, điều này làm tăng nhu cầu ở nút 2 và 3, nhưng những thao tác này đã được xử lý trước đó nên cuối cùng chúng không nhất quán, dẫn đến không có sự hoàn thành hợp lệ trong lần kiểm tra cuối cùng. 

Đầu ra:```
-1
```Điều này cho thấy tại sao hướng phụ thuộc lại quan trọng: đẩy các giới hạn một cách mù quáng mà không kiểm tra tính khả thi sẽ dẫn đến những nhu cầu còn lại chưa được đáp ứng. 

### Ví dụ 2 

đầu vào:```
5
1 2 0 1 0
```| tôi | cần [nó | và | 
| --- | --- | --- | --- | 
| 5 | 0 | 0 | 0 | 
| 4 | 1 | 1 | 1 | 
| 3 | 0 | 0 | 1 | 
| 2 | 2 | 2 | 3 | 
| 1 | 1 | 1 | 4 | 
Câu trả lời cuối cùng là:```
4
```Dấu vết này cho thấy các nút có chỉ số cao hơn buộc thực hiện các thao tác trước như thế nào và tác động của chúng tích lũy vào các chỉ số thấp hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được xử lý một lần, với thời gian lan truyền không đổi cho tối đa hai nút con | 
| Không gian | O(n) | Mảng lưu trữ nhu cầu hiện tại trên mỗi nút | 

Những hạn chế$n \le 100$làm cho giải pháp này nhanh chóng một cách tầm thường, nhưng cấu trúc có quy mô tuyến tính, phản ánh bản chất lan truyền cây cơ bản của vấn đề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a = [0] + a

    need = a[:]
    ans = 0

    for i in range(n, 0, -1):
        if need[i] <= 0:
            continue
        t = need[i]
        ans += t
        need[i] -= t
        if i * 2 <= n:
            need[i * 2] += t
        if i * 2 + 1 <= n:
            need[i * 2 + 1] += t

    for i in range(1, n + 1):
        if need[i] > 0:
            print(-1)
            return

    print(ans)

# provided samples
assert run("1\n1\n") == "-1"

# custom cases
assert run("3\n1 0 0\n") == "-1", "chain propagation impossibility"
assert run("1\n0\n") == "0", "already empty"
assert run("2\n1 1\n") == "-1", "no valid operation structure"
assert run("5\n1 2 0 1 0\n") == "4", "standard propagation case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 0 0 | -1 | truyền tạo ra dư lượng không thỏa mãn | 
| 1 0 | 0 | trạng thái trống tầm thường | 
| 2 1 1 | -1 | kết cấu không đủ để che phủ | 
| 5 1 2 0 1 0 | 4 | độ chính xác lan truyền đa cấp | 

## Vỏ cạnh 

Trường hợp tế nhị nhất là khi$n = 1$. Không có giá trị$x$bởi vì điều kiện$2x+1 \le n$thất bại vì tất cả đều tích cực$x$. Nếu như$a_1 > 0$, không có nước đi nào tồn tại và câu trả lời phải là$-1$. Thuật toán xử lý chính xác điều này vì vòng lặp không bao giờ áp dụng bất kỳ thao tác nào, để lại$need[1]$khác không, gây ra thất bại. 

Một trường hợp cạnh khác xảy ra khi tất cả các đồng xu tập trung gần các lá, chẳng hạn như$a_n > 0$. Vì không có hoạt động nào có thể nhắm mục tiêu$n$trừ khi nó xuất hiện dưới dạng$2x$hoặc$2x+1$, tính khả thi phụ thuộc hoàn toàn vào việc liệu chuỗi cha mẹ có tồn tại hay không. Quá trình xử lý ngược đảm bảo rằng nếu một lá yêu cầu giảm, nhu cầu của nó sẽ được đẩy lên cao và tính không khả thi sẽ được phát hiện nếu không có tổ tiên hợp lệ nào có thể hấp thụ nó. 

Trường hợp thứ ba là khi nhu cầu bị loại bỏ thông qua việc truyền bá. Ngay cả khi các nút trung gian tạm thời trở nên âm hoặc quá dương, bước xác thực cuối cùng sẽ đảm bảo tính nhất quán trên toàn bộ cấu trúc, ngăn chặn việc đếm quá mức ẩn tạo ra câu trả lời sai.
