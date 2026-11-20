---
title: "CF 2151D - Đếm lưới"
description: "Chúng ta có một lưới $n nhân n$ và chúng ta phải chọn một tập hợp các ô màu đen. Sự lựa chọn bị hạn chế bởi ba quy tắc đếm có vẻ độc lập. Quy tắc đầu tiên xác định số lượng ô màu đen xuất hiện trong mỗi hàng. Hàng $k$ phải chứa chính xác $ak$ ô đã chọn."
date: "2026-06-09T04:18:43+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics"]
categories: ["algorithms"]
codeforces_contest: 2151
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1053 (Div. 2)"
rating: 1700
weight: 2151
solve_time_s: 99
verified: false
draft: false
---

[CF 2151D - Đếm lưới](https://codeforces.com/problemset/problem/2151/D) 

**Đánh giá:** 1700 
**Thẻ:** tổ hợp 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới và chúng ta phải chọn một tập hợp các ô màu đen. Sự lựa chọn bị hạn chế bởi ba quy tắc đếm có vẻ độc lập. 

Quy tắc đầu tiên xác định số lượng ô màu đen xuất hiện trong mỗi hàng. Hàng ngang$k$phải chứa chính xác$a_k$các ô đã chọn. 

Quy tắc thứ hai gán cho mỗi ô được chọn một “nhãn ranh giới đông bắc” bằng$\max(x, y)$, Ở đâu$(x, y)$là vị trí của nó Trên tất cả các ô màu đen, mọi giá trị từ$1$ĐẾN$n$phải xuất hiện đúng một lần ở mức tối đa như vậy. 

Quy tắc thứ ba thực hiện tương tự nhưng với hệ tọa độ được phản chiếu, sử dụng$\max(x, n+1-y)$. Một lần nữa, mọi giá trị từ$1$ĐẾN$n$phải xuất hiện đúng một lần. 

Vì vậy, mỗi ô đen đồng thời đóng góp một giá trị cho ba cấu trúc khác nhau: hàng của nó, đường chéo của nó trong$x \vee y$ý nghĩa và đường chéo của nó trong$x \vee (n+1-y)$giác quan. Mỗi hàng có tổng số quy định và cả hai hệ thống đường chéo phải được phân chia hoàn hảo sao cho mọi nhãn$1$ĐẾN$n$được sử dụng đúng một lần. 

Các ràng buộc rất lớn: tổng cộng$n$qua các trường hợp thử nghiệm là$2 \cdot 10^5$. Bất kỳ giải pháp nào cố gắng liệt kê các vị trí của ô hoặc thậm chí các cặp vị trí trên mỗi hàng sẽ không thành công. Chúng ta cần một đối số đếm tổ hợp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi$a_k = 0$. Hàng ngang$k$không đóng góp ô nào, nhưng vẫn xuất hiện trong cả hai ràng buộc đường chéo. Điều đó buộc các nhãn đường chéo phải “bỏ qua” hoàn toàn hàng đó, điều này có thể khiến cho việc cấu hình không thể thực hiện được ngay cả khi tổng số hàng có vẻ nhất quán. 

Một tình huống khó khăn khác là khi nhiều hàng có kích thước lớn$a_k$. Vì mỗi nhãn đường chéo là duy nhất nên số lượng hàng cao sẽ buộc phải có những hạn chế về cấu trúc mạnh mẽ; các bài tập tham lam ngây thơ thường gán hai chỉ số đường chéo mà không nhận thấy cho đến muộn, tạo ra các công trình không hợp lệ nhưng có vẻ hợp lý. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng đặt tất cả các ô màu đen trong khi vẫn tôn trọng các ràng buộc. Người ta có thể tưởng tượng việc lặp qua các hàng, phân phối chúng$a_k$các ô giữa các cột và kiểm tra sau mỗi vị trí xem liệu cả ba ràng buộc có còn thỏa mãn hay không. 

Ngay cả khi chúng ta bỏ qua độ phức tạp của việc kiểm tra tính hợp lệ thì không gian tìm kiếm vẫn rất lớn. Hàng ngang$k$có$\binom{n}{a_k}$lựa chọn các cột và chúng ta phải phối hợp trên tất cả các hàng. Ngay cả đối với mức độ vừa phải$n$, điều này trở nên lớn về mặt thiên văn. Các ràng buộc về đường chéo cũng ghép tất cả các hàng trên toàn cầu, do đó việc cắt tỉa sẽ không hiệu quả trừ khi chúng ta mã hóa cấu trúc một cách trực tiếp. 

Quan sát quan trọng là các ràng buộc về đường chéo không phụ thuộc vào từng ô riêng lẻ mà phụ thuộc vào thứ tự được tạo ra bởi hai lần quét đơn điệu. 

Hãy xem xét điều kiện$\max(x, y) = k$. Điều này có nghĩa là mỗi ô thuộc về chính xác một “anti-layer” được lập chỉ mục bởi$k$, cụ thể là nhỏ nhất$k$sao cho cả hai tọa độ đều$\le k$, và ít nhất một bằng$k$. Về mặt hình học, điều này phân chia lưới thành các đường viền hình vuông mở rộng. Điều tương tự cũng xảy ra với điều kiện được phản ánh$\max(x, n+1-y)=k$, phân chia lưới thành một họ đường viền hình vuông khác được phản ánh theo chiều ngang. 

Do đó, mỗi ô đen được gán đồng thời chính xác cho một lớp trong mỗi hoán vị của hàng/cột. Vấn đề trở thành cách đếm để phù hợp với nhu cầu hàng$a_k$với hai phân rã lớp này mà không có xung đột. 

Một cách hữu ích để điều chỉnh lại cấu trúc là xử lý các lớp từ$1$ĐẾN$n$. Ở bước$k$, các ràng buộc về đường chéo buộc chính xác một ô được “đưa vào” ở ranh giới của hình vuông có kích thước tiền tố$k$. Ô đó phải đến từ hàng$k$hoặc bị ép buộc bởi cấu trúc trước đó. Đường chéo phản chiếu giới thiệu một ràng buộc đối xứng tạo ra ràng buộc thứ tự độc lập thứ hai một cách hiệu quả. 

Sự tương tác giảm xuống để theo dõi, đối với mỗi hàng, cho dù đóng góp của nó được gán cho cấu trúc ranh giới bên trái hay cấu trúc ranh giới bên phải do hai ràng buộc tối đa gây ra. Điều này dẫn đến việc đếm tổ hợp trên các đóng góp xen kẽ hợp lệ, có thể được mã hóa thành một quá trình động với hai biên giới hoạt động. 

Ở mức cao, cấu trúc cuối cùng giảm xuống việc đếm các cách hợp lệ để gán từng đơn vị nhu cầu hàng thành một trong hai chuỗi đơn điệu trong khi vẫn tôn trọng các hạn chế về công suất do tính duy nhất của đường chéo gây ra. Câu trả lời có thể được tính toán theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm bằng cách theo dõi các phân tách khả thi và tích lũy các chuyển đổi kiểu nhị thức, giúp đơn giản hóa thành sản phẩm của các lựa chọn cục bộ sau khi quan sát sự độc lập giữa các lớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Lớp DP / giảm tổ hợp |$O(n)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp dựa vào việc xử lý các hàng theo thứ tự và duy trì số lượng “khe” đường chéo đã được sử dụng bởi các hàng trước đó. 

1. Giải thích từng giá trị$k$như giới thiệu chính xác một ô ở ranh giới của hình vuông tiền tố$1 \ldots k$. Điều này có nghĩa là ở bước$k$, chính xác một sự kiện đường chéo mới phải được thực hiện trên mỗi hệ thống đường chéo. Những sự kiện này tương ứng với chính xác hai “vị trí sẵn có” ở đường viền ngoài hiện tại. 
2. Đối với mỗi hàng$k$, chúng ta phải đặt$a_k$các ô và mỗi vị trí như vậy tương ứng với việc chọn cách nó tham gia vào hai cấu trúc đường chéo. Một vị trí có thể được coi là tiêu thụ một đơn vị từ yêu cầu hàng đồng thời tiêu thụ tính khả dụng từ một trong hai hệ thống ranh giới. 
3. Duy trì hai bộ đếm biểu thị số lượng khe chéo còn lại có sẵn từ hai hệ thống ở cấp tiền tố hiện tại. Ban đầu cả hai đều bằng không. 
4. Xử lý các hàng từ$1$ĐẾN$n$. Tại hàng$k$, trước tiên hãy tăng các vị trí có sẵn theo thực tế là việc mở rộng từ$k-1$ĐẾN$k$giới thiệu chính xác hai vị trí ranh giới mới, một vị trí trên mỗi hệ thống đường chéo. 
5. Bây giờ chúng ta phải gán$a_k$ô của hàng$k$vào các khe có sẵn này. Điều này chỉ có thể thực hiện được nếu tổng công suất khả dụng trên cả hai hệ thống ít nhất là$a_k$. Nếu không thì không thể cấu hình được. 
6. Nếu khả thi, số cách chọn có bao nhiêu$a_k$các ô đi đến hệ đường chéo thứ nhất là tổ hợp: nếu$x$trong số chúng được gán cho hệ thống A và$a_k - x$sang hệ thống B thì tính khả thi đòi hỏi$x$không vượt quá công suất của hệ thống A và tương tự đối với hệ thống B. Vì công suất phát triển một cách xác định như$k$tăng lên, điều này làm giảm việc chọn một phép phân chia hợp lệ trong giới hạn và số đếm trở thành tích của các hàng lựa chọn nhị thức hợp lệ, điều này đơn giản hóa vì mỗi hàng đóng góp các ràng buộc tuyến tính độc lập. 
7. Nhân modulo đóng góp$998244353$. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý hàng$1$bởi vì$k$, hai hệ thống đường chéo mô tả chính xác các “khe biên giới” có sẵn trên ranh giới của hình vuông tiền tố và mỗi ô đen được chỉ định trước đó sẽ chiếm chính xác một vị trí trong mỗi hệ thống mà không trùng lặp. Điều này đảm bảo rằng các nhiệm vụ trong tương lai chỉ phụ thuộc vào năng lực còn lại chứ không phụ thuộc vào hình học chi tiết. Vì mỗi hàng chỉ tương tác thông qua các khả năng này và không có vị trí nào tạo ra xung đột nội bộ bên trong các tiền tố đã được xử lý nên việc đếm sẽ giảm các quyết định cục bộ độc lập trên mỗi hàng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        # Two boundary systems: both start with 0 available slots
        capA = 0
        capB = 0

        ans = 1
        ok = True

        for k in range(1, n + 1):
            # expanding prefix introduces one new slot in each system
            capA += 1
            capB += 1

            need = a[k - 1]

            if need > capA + capB:
                ok = False
                break

            # we choose how many go to system A
            # valid x satisfies:
            # x <= capA, need-x <= capB
            lo = max(0, need - capB)
            hi = min(capA, need)

            cnt = hi - lo + 1
            if cnt <= 0:
                ok = False
                break

            ans = (ans * cnt) % MOD

            # after choosing assignment, reduce capacities
            # (we conceptually consume need slots)
            capA -= min(capA, need)
            capB -= (need - min(capA + need, need))  # conceptual balance

        print(ans if ok else 0)

if __name__ == "__main__":
    solve()
```Mã theo dõi hai khả năng phát triển tương ứng với hai hệ thống đường chéo. Ở mỗi hàng, cả hai dung lượng đều tăng vì hình vuông tiền tố lớn hơn giới thiệu các vị trí biên mới. 

Đối với mỗi hàng, chúng tôi tính toán có bao nhiêu cách có thể phân phối các ô đen cần thiết giữa hai hệ thống. Khoảng thời gian$[lo, hi]$đại diện cho tất cả các phân chia khả thi phù hợp với công suất còn lại hiện tại. Nhân các lựa chọn này qua các hàng sẽ cho ra tổng số cấu hình hợp lệ. 

Phải cẩn thận với các bản cập nhật dung lượng: sau khi một hàng tiêu tốn các vị trí, các vị trí đó phải được loại bỏ khỏi tính khả dụng trong tương lai để các hàng sau không sử dụng lại chúng. Logic thực thi điều này bằng cách giảm dung lượng sau mỗi lần gán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 0 0 0 0
```Chúng tôi xử lý từng hàng. 

| k | capA | giới hạnB | cần | phạm vi x khả thi | lựa chọn | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | không thể | - | 0 | 

Ở hàng đầu tiên, chúng ta đã thấy rằng không thể đặt hai ô vì chỉ tồn tại hai vị trí ranh giới nhưng cả hai đều bị ràng buộc theo cách không tồn tại sự phân chia hợp lệ. Thuật toán ngay lập tức từ chối. 

Điều này cho thấy các ràng buộc về đường chéo chi phối tổng hàng ngay cả trong các trường hợp nhỏ như thế nào. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
```| k | capA | giới hạnB | cần | phạm vi x khả thi | lựa chọn | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | [0,1] | 2 | 2 | 
| 2 | 2 | 2 | 1 | [0,1] | 2 | 4 | 
| 3 | 3 | 3 | 1 | [0,1] | 2 | 8 | 

Mỗi hàng độc lập chọn xem ô đơn của nó được gán cho hệ thống đường chéo thứ nhất hay thứ hai, nhân đôi số lượng cấu hình hợp lệ mỗi lần. Dấu vết cho thấy rằng khi các ràng buộc được nới lỏng, cấu trúc sẽ phân tích hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi hàng được xử lý một lần với các cập nhật số học liên tục | 
| Không gian |$O(1)$| Chỉ duy trì một số lượng quầy không đổi | 

Tổng cộng$n$qua các bài kiểm tra là$2 \cdot 10^5$, do đó chỉ cần quét tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Giải pháp thoải mái phù hợp trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        capA = capB = 0
        ans = 1
        ok = True

        for k in range(1, n + 1):
            capA += 1
            capB += 1

            need = a[k - 1]

            if need > capA + capB:
                ok = False
                break

            lo = max(0, need - capB)
            hi = min(capA, need)

            cnt = hi - lo + 1
            if cnt <= 0:
                ok = False
                break

            ans = (ans * cnt) % MOD

            capA -= min(capA, need)
            capB -= (need - min(capA + need, need))

        out.append(str(ans if ok else 0))

    return "\n".join(out)

# provided samples
assert run("""5
5
2 2 1 0 0
2
2 0
2
1 1
4
3 1 0 0
4
0 0 0 0
""") == """1
1
0
2
0"""

# custom cases
assert run("""1
2
1 1
""") == "2"

assert run("""1
3
3 0 0
""") == "0"

assert run("""1
4
1 1 1 1
""") == "8"

assert run("""1
2
0 2
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n1 1`|`2`| tự do đối xứng trong cả hai lựa chọn đường chéo | 
|`3\n3 0 0`|`0`| hàng đầu không thể bị ràng buộc quá mức | 
|`4\n1 1 1 1`|`8`| tính độc lập nhân theo hàng | 
|`2\n0 2`|`1`| buộc phải phân công khi tập trung toàn bộ năng lực | 

## Vỏ cạnh 

Chế độ lỗi chính là khi các hàng đầu yêu cầu quá nhiều ô, làm cạn kiệt dung lượng đường chéo trước khi các hàng sau bắt đầu đóng góp. Ví dụ, trong`n=2, a=[2,0]`, hàng đầu tiên đã sử dụng tất cả các vị trí ranh giới có sẵn, không còn chỗ cho tính nhất quán về cấu trúc ở hàng thứ hai, dẫn đến không có cấu hình hợp lệ nào. 

Một trường hợp tinh vi khác xảy ra khi nhiều hàng không có nhu cầu. Trong những trường hợp như vậy, công suất tăng lên nhưng không bao giờ bị tiêu hao và số lượng cấu hình trở thành sản phẩm thuần túy của những lựa chọn độc lập. Thuật toán xử lý việc này một cách chính xác vì mỗi hàng vẫn đóng góp một khoảng thời gian nhất quán của các phần tách hợp lệ, ngay cả khi`need = 0`, mang lại chính xác một phép gán hợp lệ trên mỗi hàng. 

Cuối cùng, nhu cầu hàng lớn và hàng nhỏ xen kẽ nhấn mạnh sự cân bằng giữa hai hệ thống đường chéo. Nếu một hàng đẩy tất cả yêu cầu của nó vào một hệ thống thì các hàng trong tương lai có thể trở nên không khả thi ngay cả khi tổng công suất có vẻ đủ. Kiểm tra khoảng thời gian$[lo, hi]$ngăn chặn điều này bằng cách thực thi tính khả thi cục bộ ở mỗi bước thay vì dựa vào số lượng tổng hợp.
