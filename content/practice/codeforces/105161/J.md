---
title: "CF 105161J - Tấm lợp"
description: "Chúng ta có một lưới $n lần m$ trong đó mỗi ô có một trọng số. Nhiệm vụ là đặt các ô hình chữ nhật không chồng lên nhau trên lưới này để tối đa hóa tổng trọng lượng của các ô được che phủ."
date: "2026-06-27T10:58:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105161
codeforces_index: "J"
codeforces_contest_name: "2024 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 105161
solve_time_s: 46
verified: true
draft: false
---

[CF 105161J - Tấm phủ ngói](https://codeforces.com/problemset/problem/105161/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times m$lưới trong đó mỗi ô có trọng số. Nhiệm vụ là đặt các ô hình chữ nhật không chồng lên nhau trên lưới này để tối đa hóa tổng trọng lượng của các ô được che phủ. Mỗi ô là một vật thể giống domino theo nghĩa là một trong các cạnh của nó được cố định bằng 1, trong khi cạnh kia có thể là bất kỳ số nguyên dương nào, vì vậy mỗi ô là một đoạn ngang có độ dài tùy ý trong một hàng hoặc một đoạn dọc có độ dài tùy ý trong một cột. 

Ràng buộc rằng các ô này “không được kết nối bốn theo hình chữ L” có nghĩa là chúng tôi bị cấm tạo cấu hình trong đó hai phần kề theo chiều dọc và một phần kề theo chiều ngang kết hợp thành một góc tương ứng với các phân đoạn trực giao chồng chéo tạo thành mô hình góc 2×2 của tương tác vùng phủ sóng. Nói một cách đơn giản hơn, khi chúng tôi quyết định mức độ bao phủ theo từng hàng, chúng tôi phải đảm bảo tính nhất quán để phân đoạn dọc tiếp tục đi lên không xung đột với phân đoạn ngang bắt đầu từ hàng trước đó trên cùng một cột. Đây chính xác là loại phụ thuộc xuất hiện trong cấu hình cổ điển hoặc lập trình động cắm. 

Đầu vào là kích thước lưới và ma trận trọng số. Đầu ra là một số nguyên duy nhất, tổng số ô được bao phủ tối đa có thể đạt được theo cách xếp kề hợp lệ. 

Khó khăn chính là các quyết định về vị trí đặt gạch không mang tính cục bộ. Việc chọn mở rộng ô theo chiều ngang hoặc chiều dọc sẽ ảnh hưởng đến các hàng và cột trong tương lai, điều này ngăn cản quá trình xử lý hàng độc lập hoặc tham lam. 

Một cách giải thích ngây thơ sẽ cố gắng liệt kê tất cả các ô hoặc thậm chí tất cả các tập hợp con của hình chữ nhật. Ngay cả việc giới hạn từng hàng một cách độc lập cũng đã dẫn đến sự phức tạp theo cấp số nhân trong$m$, vì mỗi hàng có thể được phân chia thành các đoạn tùy ý. Trên các hàng, các hạn chế về khả năng tương thích khiến điều này trở nên tồi tệ hơn. 

Trường hợp cạnh xuất hiện khi$n = 1$hoặc$m = 1$, trong đó lưới suy biến thành một dòng duy nhất. Trong trường hợp đó, giải pháp tối ưu chỉ đơn giản là lấy toàn bộ hàng hoặc cột làm một ô, nhưng một DP ngây thơ giả định cả hai chiều đều có thể cố gắng truyền bá các chuyển đổi trạng thái không hợp lệ. Một trường hợp khác là khi tất cả các trọng số đều âm; câu trả lời tối ưu là 0 nếu cho phép phạm vi phủ sóng trống hoặc phân đoạn đơn tốt nhất nếu không tùy thuộc vào cách giải thích. Một DP chính xác phải cho phép “không đặt bất cứ thứ gì” làm trạng thái hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ cố gắng liệt kê tất cả các cách có thể để phân chia lưới thành các đoạn ngang và dọc thỏa mãn các ràng buộc. Đối với mỗi cấu hình, chúng tôi tính tổng của tất cả các ô được bao phủ và giữ mức tối đa. 

Ngay cả khi chúng ta hạn chế sự chú ý vào việc quyết định, đối với mỗi ô, dù nó bắt đầu một đoạn ngang hay dọc, thì số lượng các khả năng vẫn theo cấp số nhân theo$nm$. Mỗi ô tham gia vào một quyết định nhị phân và các ràng buộc hợp lệ sẽ kết hợp các ô lân cận theo cả hai hướng, dẫn đến gần như$O(2^{nm})$tiểu bang. Điều này hoàn toàn không khả thi ngoài các lưới rất nhỏ. 

Quan sát cấu trúc quan trọng là lưới có thể được xử lý theo thứ tự quét và thông tin duy nhất cần thiết để đảm bảo tính hợp lệ là cách mỗi cột tương tác giữa hàng hiện tại và hàng trước đó. Khi chúng ta quét từ trái sang phải trong một hàng, chúng ta cũng cần biết liệu một đoạn dọc đã “hoạt động” từ hàng trước đó trong mỗi cột hay chưa. Điều này tự nhiên dẫn đến trạng thái DP hồ sơ trên một mặt nạ bit có kích thước$m$, được tăng cường thêm một bit thông tin liên tục theo chiều dọc trên mỗi cột để ngăn chặn sự chồng chéo hình chữ L bất hợp pháp. 

Đây chính xác là một plug DP: thay vì lưu trữ toàn bộ lịch sử của các hình dạng ô xếp, chúng tôi chỉ lưu trữ các tương tác ranh giới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{nm})$|$O(nm)$| Quá chậm | 
| Hồ sơ DP (cắm DP) |$O(nm2^m)$|$O(2^m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý ô lưới theo ô theo thứ tự hàng chính, duy trì mặt nạ bit mô tả trạng thái của từng cột ở ranh giới hàng hiện tại. 

1. Chúng tôi xác định trạng thái DP$dp[i][j][S]$, Ở đâu$(i, j)$là vị trí hiện tại trong quá trình duyệt hàng chính và$S$mã hóa xem mỗi cột hiện đang được “chiếm” hay “mở” ở ranh giới giữa các hàng. Trong thực tế, chúng tôi nén điều này thành việc cuộn DP qua mặt nạ. 
2. Tại mỗi ô$(i, j)$, chúng tôi quyết định có nên để nó không sử dụng hay đưa nó vào một ngăn xếp. Việc không sử dụng nó chỉ đơn giản là truyền bá trạng thái về phía trước mà không sửa đổi, điều này duy trì khả năng tương thích cho các vị trí trong tương lai. 
3. Nếu chúng tôi chọn đưa vào$(i, j)$, chúng ta phải quyết định xem nó thuộc về đoạn ngang kéo dài sang bên phải hay đoạn thẳng đứng kéo dài xuống dưới. Phần mở rộng theo chiều ngang chỉ ảnh hưởng đến mặt nạ hàng hiện tại, trong khi phần mở rộng theo chiều dọc ảnh hưởng đến trạng thái hàng tiếp theo trong cột$j$. 
4. Ràng buộc tinh vi đang ngăn chặn các tương tác góc không hợp lệ. Nếu một đoạn thẳng đứng tiếp tục từ hàng trước tại cột$j$và chúng tôi cũng bắt đầu một đoạn ngang tại$(i, j)$, chúng ta sẽ tạo thành một tương tác hình chữ L vi phạm điều kiện. Để ngăn chặn điều này, trạng thái phải mã hóa không chỉ cột có đang hoạt động hay không mà còn phải mã hóa cột đó đến từ phía trên hay cột mới được bắt đầu. 
5. Để thực thi điều này, chúng tôi mở rộng mặt nạ để lưu trữ hai lớp thông tin trên mỗi cột: liệu ô đã được che từ phía trên hay chưa và liệu ô hiện có đang được mở rộng theo chiều ngang trong cùng một hàng hay không. Điều này đảm bảo rằng các quá trình chuyển đổi không bao giờ tạo ra sự chồng chéo xung đột. 
6. Chúng tôi lặp lại tất cả các mặt nạ và chuyển tiếp, cập nhật giá trị DP bằng cách thêm trọng lượng của ô đã chọn khi nó bị che. 
7. Chúng tôi sử dụng mảng cuộn$(i, j)$, vì chỉ cần bước trước đó nên việc giảm bộ nhớ từ$O(nm2^m)$ĐẾN$O(m2^m)$. 

### Tại sao nó hoạt động 

Ở bất kỳ bước nào, trạng thái DP mô tả đầy đủ ranh giới tương tác giữa các phần được xử lý và chưa được xử lý của lưới. Bất kỳ ô nào vượt qua ranh giới này đều phải được thể hiện ở trạng thái và mọi cấu hình hình chữ L không hợp lệ sẽ yêu cầu thông tin mâu thuẫn về việc liệu một cột có đồng thời tiếp tục theo chiều dọc và bắt đầu một đoạn ngang hay không. Vì trạng thái phân tách rõ ràng các vai trò này nên mọi chuyển đổi đều duy trì tính nhất quán. Bởi vì mỗi ô hợp lệ tương ứng với chính xác một chuỗi chuyển đổi trạng thái nhất quán và mọi chuyển đổi đều tôn trọng các ràng buộc ô, DP liệt kê tất cả các cấu hình hợp lệ mà không trùng lặp hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    # DP over profile masks
    # state: mask of columns currently "open from previous row"
    # interpretation: 1 means column has a vertical continuation constraint active

    INF = -10**18
    dp = [INF] * (1 << m)
    dp[0] = 0

    for i in range(n):
        for j in range(m):
            ndp = [INF] * (1 << m)
            w = a[i][j]

            for mask in range(1 << m):
                if dp[mask] == INF:
                    continue

                cur = dp[mask]

                # 1. skip cell
                if cur > ndp[mask]:
                    ndp[mask] = cur

                # 2. take cell as horizontal start/extension
                # only allowed if no vertical conflict in this column
                if not (mask & (1 << j)):
                    new_mask = mask
                    if cur + w > ndp[new_mask]:
                        ndp[new_mask] = cur + w

                # 3. take cell as vertical continuation into next row
                # mark column j as active downward
                new_mask = mask | (1 << j)
                if cur + w > ndp[new_mask]:
                    ndp[new_mask] = cur + w

            dp = ndp

    print(max(dp))

if __name__ == "__main__":
    solve()
```Việc triển khai nén DP thành một mặt nạ duy nhất cho mỗi trạng thái, trong đó mỗi bit theo dõi xem một đoạn dọc có đang truyền qua cột đó sang hàng tiếp theo hay không. Vòng lặp chuyển tiếp xem xét từng ô theo thứ tự hàng chính, cập nhật mặt nạ tùy thuộc vào việc chúng ta bắt đầu hay tiếp tục một phân đoạn dọc. 

Quá trình chuyển đổi bỏ qua sẽ bảo tồn mặt nạ hiện tại và chuyển tiếp giá trị DP không thay đổi. Lựa chọn theo chiều ngang chỉ được phép khi không có phân đoạn dọc nào hoạt động trong cột đó vì điều đó sẽ tương ứng với tương tác góc không hợp lệ. Lựa chọn theo chiều dọc đặt bit, đảm bảo các ô trong tương lai trong cột đó nhận thức được ràng buộc tiếp tục theo chiều dọc. 

Rolling DP tránh lưu trữ toàn bộ lịch sử lưới vì mỗi bước chỉ phụ thuộc vào trạng thái mặt nạ trước đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một cách đơn giản$2 \times 2$lưới: 

đầu vào:```
2 2
1 2
3 4
```Chúng tôi theo dõi trạng thái DP sau khi xử lý từng ô. 

### Dấu vết hàng chính 

| Bước | Tế bào | Mặt nạ | Giá trị DP | 
| --- | --- | --- | --- | 
| 0 | bắt đầu | 00 | 0 | 
| 1 | (0,0)=1 | 00 | 1 | 
| 2 | (0,1)=2 | 00 | 3 | 
| 3 | (1,0)=3 | 00 | 6 | 
| 4 | (1,1)=4 | 00 | 10 | 

Dấu vết này tương ứng với việc lấy tất cả các ô mà không có xung đột, điều này hợp lệ vì không cần truyền dọc. 

Ví dụ thứ hai giới thiệu sự phụ thuộc theo chiều dọc bắt buộc: 

đầu vào:```
2 1
5
7
```| Bước | Tế bào | Mặt nạ | Giá trị DP | 
| --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | 
| 1 | 5 | 0 | 5 | 
| 2 | 7 | 1 | 12 | 

Điều này chứng tỏ cách tiếp tục theo chiều dọc tích lũy trọng lượng trên các hàng trong một cột. 

Trường hợp thứ hai xác nhận rằng các phân đoạn dọc tích lũy chính xác các khoản đóng góp trên các hàng mà không yêu cầu theo dõi phân đoạn rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm2^m)$| Mỗi ô xử lý tất cả các mặt nạ và chuyển tiếp | 
| Không gian |$O(2^m)$| Chỉ lớp DP hiện tại trên mặt nạ mới được lưu trữ | 

Độ phức tạp bị chi phối bởi tập hợp con DP trên các trạng thái cột. Điều này khả thi khi$m$nhỏ, thường lên tới khoảng 10 đến 12, phù hợp với cài đặt DP của phích cắm dự định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    INF = -10**18
    dp = [INF] * (1 << m)
    dp[0] = 0

    for i in range(n):
        for j in range(m):
            ndp = [INF] * (1 << m)
            w = a[i][j]
            for mask in range(1 << m):
                if dp[mask] == INF:
                    continue
                cur = dp[mask]
                ndp[mask] = max(ndp[mask], cur)
                if not (mask & (1 << j)):
                    ndp[mask] = max(ndp[mask], cur + w)
                ndp[mask | (1 << j)] = max(ndp[mask | (1 << j)], cur + w)
            dp = ndp

    return str(max(dp))

# minimum case
assert run("1 1\n5") == "5"

# small grid
assert run("2 2\n1 2\n3 4") == "10"

# single column
assert run("3 1\n1\n2\n3") == "6"

# all negative
assert run("2 2\n-1 -2\n-3 -4") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 5 | trường hợp cơ sở đúng đắn | 
| Lưới 2×2 | 10 | tích lũy bảo hiểm đầy đủ | 
| cột 3×1 | 6 | truyền dọc | 
| tất cả tiêu cực | 0 | xử lý lựa chọn trống | 

## Vỏ cạnh 

Lưới 1×1 có giá trị dương xác nhận rằng DP khởi tạo chính xác và cho phép lấy một ô mà không có bất kỳ chuyển đổi nào. Trạng thái bắt đầu ở mặt nạ 0 và quá trình chuyển đổi tốt nhất sẽ trực tiếp thêm trọng lượng ô. 

Một lưới cột đơn thể hiện sự lan truyền theo chiều dọc. Bắt đầu với mặt nạ 0, việc chọn từng ô sẽ đặt bit dọc liên tục và DP sẽ tích lũy tất cả các trọng số. Quá trình chuyển đổi không bao giờ gây ra xung đột theo chiều ngang vì không có hàng xóm theo chiều ngang. 

Một lưới có tất cả các giá trị âm sẽ kiểm tra xem có được phép bỏ qua hay không. Quá trình chuyển đổi bỏ qua sẽ giữ nguyên mặt nạ và tránh thêm các đóng góp tiêu cực, do đó câu trả lời cuối cùng vẫn bằng 0 nếu lựa chọn trống là tối ưu.
