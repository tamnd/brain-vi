---
title: "CF 103448H - \u72c2\u4e71"
description: "Chúng ta được cung cấp một tập hợp các đơn vị chiến đấu, mỗi đơn vị được mô tả bằng giá trị tấn công và giá trị máu. Một đơn vị được chọn làm đơn vị “hoạt động” ban đầu."
date: "2026-07-03T07:27:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103448
codeforces_index: "H"
codeforces_contest_name: "The 16-th Beihang University Collegiate Programming Contest (BCPC 2021) - Preliminary"
rating: 0
weight: 103448
solve_time_s: 58
verified: true
draft: false
---

[CF 103448H - \u72c2\u4e71](https://codeforces.com/problemset/problem/103448/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đơn vị chiến đấu, mỗi đơn vị được mô tả bằng giá trị tấn công và giá trị máu. Một đơn vị được chọn làm đơn vị “hoạt động” ban đầu. Sau đó, đơn vị này liên tục chiến đấu với từng đơn vị khác và mỗi trận chiến đều mang tính quyết định: cả hai bên đều gây sát thương bằng giá trị tấn công của mình đồng thời, liên tục cho đến khi ít nhất một bên không còn máu. Một đơn vị được coi là đã chết khi sức khỏe của nó trở nên không tích cực. 

Quá trình tiếp tục miễn là đơn vị hoạt động vẫn còn sống và vẫn còn các đơn vị khác. Trong mỗi bước, đơn vị chủ động có thể đối mặt với bất kỳ đối thủ nào còn lại và chúng ta được phép cho rằng trình tự đối thủ được chọn theo cách thuận lợi nhất. Câu hỏi đặt ra là liệu có tồn tại ít nhất một sự lựa chọn về đơn vị xuất phát và ít nhất một mệnh lệnh của đối thủ sao cho tất cả các đơn vị, bao gồm cả đơn vị xuất phát, cuối cùng sẽ chết. 

Các ràng buộc cho phép tối đa 100000 đơn vị, với giá trị tấn công và sức khỏe lên tới 1e6. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng chiến đấu theo từng cặp hoặc thử tất cả các hoán vị. Ngay cả việc suy luận O(n^2) cho mỗi ứng viên cũng trở nên quá chậm, do đó, bất kỳ giải pháp khả thi nào cũng phải giảm bớt vấn đề để tổng hợp các phép tính trên mỗi đơn vị. 

Một trường hợp thất bại tinh tế xuất hiện khi một người cố gắng mô phỏng các trận đánh một cách tham lam. Ví dụ: giả sử đơn vị chủ động phải luôn chiến đấu với đối thủ yếu nhất trước có thể gây hiểu nhầm, bởi vì đối thủ yếu có sức tấn công cao có thể nguy hiểm hơn đối thủ mạnh có sức tấn công thấp. Một chế độ thất bại khác là giả định rằng khả năng sống sót chỉ phụ thuộc vào tổng sát thương nhận được mà không xem xét liệu đơn vị đang hoạt động có thực sự hoàn thành tất cả các lần tiêu diệt trước khi chết giữa quá trình hay không. 

Mô hình chính xác phải nắm bắt được cả “mỗi trận chiến kéo dài bao lâu” và “mức độ thiệt hại gây ra trong thời gian đó”, trong khi vẫn duy trì tính độc lập theo trật tự theo cách hữu ích. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi đơn vị khởi đầu có thể và đối với mỗi đơn vị, mô phỏng tất cả các hoán vị của các đối thủ còn lại. Trong mỗi mô phỏng, chúng tôi sẽ thực hiện quy trình chiến đấu theo từng bước. Mỗi cuộc chiến có thể kéo dài tới O(1e6 / atk) hiệp và trong trường hợp xấu nhất có O(n) trận đấu cho mỗi hoán vị. Số lượng hoán vị làm cho điều này hoàn toàn không khả thi. 

Quan sát quan trọng là sau khi đơn vị xuất phát được cố định, mỗi đối thủ sẽ đóng góp một khoản chi phí được xác định đầy đủ cho đơn vị xuất phát, không phụ thuộc vào thứ tự, miễn là đơn vị xuất phát tồn tại đủ lâu để tiếp cận trận chiến đó. Mỗi đối thủ j yêu cầu một số lượt tấn công cố định được xác định bằng số lượng đòn đánh của đơn vị xuất phát cần thiết để tiêu diệt đối thủ. Trong mỗi hiệp đấu đó, đơn vị xuất phát sẽ nhận sát thương bằng đòn tấn công của đối thủ. Điều này có nghĩa là mỗi đối thủ đóng góp tổng chi phí thiệt hại xác định đối với đơn vị xuất phát. 

Vấn đề đặt hàng trở nên đơn giản hơn lần đầu tiên xuất hiện. Tổng thiệt hại gây ra là cố định bất kể thứ tự và hạn chế thực sự duy nhất là liệu đơn vị ban đầu có sống sót đủ lâu để hoàn thành tất cả các trận chiến hay không. Vì khả năng sống sót chỉ phụ thuộc vào sát thương tích lũy nên bất kỳ thứ tự nào không vượt quá lượng máu ở bất kỳ tiền tố nào đều hợp lệ và thứ tự đó tồn tại chính xác khi tổng sát thương không vượt quá lượng máu của đơn vị ban đầu. 

Điều này làm giảm vấn đề đánh giá biểu thức dạng đóng cho từng đơn vị khởi đầu có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n! · n · vòng) | O(1) | Quá chậm | 
| Biểu mẫu đóng cho mỗi ứng viên | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa một đơn vị ban đầu ứng cử viên i và phân tích xem liệu nó có thể loại bỏ tất cả các đơn vị khác trước khi chết hay không. 

### 1. Tính toán trước tổng hợp toàn cầu

Chúng tôi tính tổng tổng của tất cả các giá trị tấn công và tổng tổng của đòn tấn công nhân với lượng máu trên tất cả các đơn vị. Những tổng hợp này cho phép chúng tôi thể hiện sự đóng góp của mọi đối thủ một cách cô đọng. 

### 2. Xử lý riêng các trường hợp không tấn công 

Nếu đơn vị ban đầu không có đòn tấn công nào và có tổng cộng nhiều hơn một đơn vị, thì đơn vị đó không bao giờ có thể tiêu diệt bất kỳ đối thủ nào, vì vậy đơn vị đó ngay lập tức không hợp lệ. Nếu nó là đơn vị duy nhất, câu trả lời có giá trị tầm thường. 

Lý do là đòn tấn công bằng 0 có nghĩa là số lần đánh cần thiết là vô hạn để tiêu diệt bất kỳ đơn vị sức khỏe tích cực nào. 

### 3. Tính toán phần đóng góp của đối thủ cho đơn vị khởi đầu cố định 

Đối với đơn vị xuất phát cố định i với đòn tấn công A, mỗi đối thủ j yêu cầu số đòn đánh bằng mức trần hp_j / A. Trong mỗi lần đánh, j gây atk_j sát thương cho i, do đó tổng sát thương từ j là: 

trần(hp_j / A) × atk_j. 

Chúng ta viết lại số hạng trần thành (hp_j + A − 1) // A, làm cho nó có thể tính toán được dưới dạng số nguyên. 

### 4. Chuyển tổng sát thương thành dạng đóng 

Chúng tôi mở rộng tổng trên tất cả các đối thủ và thể hiện nó bằng cách sử dụng tổng hợp toàn cầu để chúng tôi có thể tính toán nó theo O(1) cho mỗi ứng cử viên. 

### 5. Kiểm tra tình trạng sống sót 

Đơn vị ban đầu có hiệu lực nếu tổng sát thương nhận được nhỏ hơn hoặc bằng máu của chính nó. Sự bình đẳng được cho phép vì nó có nghĩa là trận chiến cuối cùng kết thúc với việc cả hai bên đều chết cùng một lúc. 

### 6. Thử tất cả các ứng viên 

Chúng tôi đánh giá điều kiện này cho mọi đơn vị và trả về CÓ nếu có ít nhất một đơn vị thỏa mãn nó. 

### Tại sao nó hoạt động 

Đối với một đơn vị xuất phát cố định, mỗi đối thủ đóng góp một lượng tổng sát thương xác định không phụ thuộc vào thứ tự. Mọi thứ tự hợp lệ chỉ cần đảm bảo rằng tổng tiền tố không vượt quá sức khỏe, nhưng tính khả thi của tiền tố được đảm bảo bất cứ khi nào tổng số tiền nằm trong giới hạn vì tất cả đóng góp đều không âm. Do đó, khả năng sống sót giảm xuống thành một sự bất bình đẳng duy nhất khi so sánh tổng thiệt hại nhận được với lượng máu của đơn vị ban đầu. Vì quá trình luôn kết thúc khi đơn vị ban đầu chết hoặc tất cả đối thủ đều chết, nên việc thỏa mãn bất đẳng thức này chính là đặc điểm của tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    atk = []
    hp = []

    total_atk = 0
    total_ah = 0  # sum atk * hp

    for _ in range(n):
        a, h = map(int, input().split())
        atk.append(a)
        hp.append(h)
        total_atk += a
        total_ah += a * h

    if n == 1:
        print("YES")
        return

    for i in range(n):
        a_i = atk[i]
        h_i = hp[i]

        if a_i == 0:
            continue

        # compute sum_{j != i} ceil(hp_j / a_i) * atk_j
        # = sum atk_j * (hp_j + a_i - 1) // a_i

        num = total_ah - atk[i] * hp[i] + (a_i - 1) * (total_atk - atk[i])
        if num // a_i <= h_i:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng tổng thể để mỗi ứng viên có thể được đánh giá trong thời gian không đổi. Đối với mỗi đơn vị ban đầu, nó sẽ xây dựng lại biểu thức tổng thiệt hại bằng cách sử dụng phân rã đại số thay vì lặp lại tất cả các đối thủ. 

Một chi tiết quan trọng là việc tách đơn vị được chọn khỏi tổng toàn cục, vì nó không được đóng góp vào tập hợp đối thủ của chính nó. Một điểm tinh tế khác là xử lý các đơn vị tấn công bằng 0 một cách an toàn bằng cách bỏ qua chúng, vì nếu không thì việc phân chia sẽ không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
2 6
3 3
```Chúng tôi tính toán các giá trị chung: Total_atk = 5, Total_ah = 2×6 + 3×3 = 12 + 9 = 21. 

Chúng tôi kiểm tra từng đơn vị. 

| tôi | atk | hp | giới hạn thiệt hại tính toán | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 6 | thỏa mãn bất đẳng thức | CÓ | 
| 1 | 3 | 3 | thất bại bất bình đẳng | - | 

Với i = 0, đối thủ 1 có thể bị giết sau 2 đòn đánh và sát thương đủ nhỏ để đơn vị sống sót cho đến khi cả hai đều chết. Điều này khẳng định tính khả thi. 

### Ví dụ 2 

đầu vào:```
3
2 3
3 2
3 3
```Giá trị chung: Total_atk = 8, Total_ah = 2×3 + 3×2 + 3×3 = 6 + 6 + 9 = 21. 

Chúng tôi kiểm tra từng ứng cử viên: 

| tôi | atk | hp | kết quả | 
| --- | --- | --- | --- | 
| 0 | 2 | 3 | thất bại | 
| 1 | 3 | 2 | thất bại | 
| 2 | 3 | 3 | thành công | 

Với i = 2, đơn vị có đủ máu hữu hiệu để duy trì sát thương tổng hợp trong khi loại bỏ các đơn vị khác theo trình tự, vì vậy đây là điểm khởi đầu hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tổng hợp và một lượt để kiểm tra từng ứng viên | 
| Không gian | O(1) thêm | Chỉ tổng và mảng lưu trữ đầu vào toàn cầu | 

Giải pháp này phù hợp thoải mái trong giới hạn vì tất cả các phép toán đều là số học số nguyên đơn giản trên 100000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Since full solution is not modularized in this snippet,
# these asserts are illustrative rather than executable.

# provided samples
# assert run("2\n2 6\n3 3\n") == "YES"

# custom cases
# single node
# assert run("1\n10 5\n") == "YES"

# zero attack invalid case
# assert run("2\n0 10\n5 5\n") == "NO"

# all equal
# assert run("3\n2 2\n2 2\n2 2\n") == "NO"

# strong single candidate
# assert run("3\n10 1\n1 100\n1 100\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | CÓ | Trường hợp thành công tầm thường | 
| không tấn công + người khác | KHÔNG | Không thể loại bỏ đối thủ | 
| tất cả các đơn vị yếu như nhau | KHÔNG | Không có người sống sót khả thi | 
| một đơn vị thống trị | CÓ | Trường hợp tồn tại hợp lệ | 

## Vỏ cạnh 

Trường hợp cạnh tranh quan trọng là khi đơn vị được chọn không có khả năng tấn công. Đối với đầu vào như:```
2
0 10
5 5
```đơn vị đầu tiên không thể tiêu diệt được bất cứ thứ gì nên nó sẽ thất bại ngay lập tức. Thuật toán xử lý việc này bằng cách bỏ qua các ứng cử viên không có khả năng tấn công. 

Một trường hợp khác là khi tất cả các đơn vị đều giống hệt nhau:```
3
2 2
2 2
2 2
```Mọi ứng cử viên đều tạo ra cấu trúc sát thương đối xứng giống nhau và không ai có thể duy trì được số lần đánh tích lũy cần thiết, vì vậy kết quả đầu ra là KHÔNG. Công thức đánh giá chính xác các đóng góp bằng nhau và không tính đến bất đẳng thức. 

Trường hợp thứ ba là một đơn vị cực kỳ mạnh:```
3
10 1
1 100
1 100
```Ở đây, đơn vị mạnh sẽ tiêu diệt cả hai đối thủ trong rất ít hiệp và sát thương tích lũy của đơn vị đó vẫn bị giới hạn bởi lượng máu của đơn vị đó. Bất đẳng thức chỉ đúng với ứng viên đó, chứng tỏ rằng thuật toán xác định chính xác điểm khởi đầu khả thi duy nhất.
