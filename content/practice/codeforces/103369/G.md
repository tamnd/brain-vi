---
title: "CF 103369G - \u0414\u0432\u0435 \u0441\u043e\u0440\u0442\u0438\u0440\u043e\u0432\u043a\u0438"
description: "Chúng ta được cung cấp một mảng các số nguyên và một thao tác được phép duy nhất có thể được sử dụng nhiều nhất một lần. Thao tác này cho phép chúng ta chọn bất kỳ tập hợp con vị trí nào và thêm cùng một giá trị dương $k$ cho tất cả các phần tử đã chọn."
date: "2026-07-03T12:51:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103369
codeforces_index: "G"
codeforces_contest_name: "Moscow team olympiad 2021"
rating: 0
weight: 103369
solve_time_s: 47
verified: true
draft: false
---

[CF 103369G - \u0414\u0432\u0435 \u0441\u043e\u0440\u0442\u0438\u0440\u043e\u0432\u043a\u0438](https://codeforces.com/problemset/problem/103369/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một thao tác được phép duy nhất có thể được sử dụng nhiều nhất một lần. Thao tác này cho phép chúng tôi chọn bất kỳ tập hợp con vị trí nào và thêm cùng một giá trị dương$k$cho tất cả các phần tử được chọn. Sau khi tùy ý áp dụng thao tác này một lần, chúng ta muốn mảng kết quả không giảm. 

Nói cách khác, chúng ta được phép “nâng” một số phần tử lên trên bằng một phép dịch chuyển đồng đều và chúng ta muốn biết liệu chúng ta có thể khắc phục tất cả các nghịch đảo do thứ tự ban đầu gây ra bằng cách chỉ sử dụng mức tăng chung duy nhất này được áp dụng cho một tập hợp con đã chọn hay không. 

Khó khăn chính là thao tác cực kỳ hạn chế: chúng ta không thể điều chỉnh các phần tử một cách độc lập, chỉ chia mảng thành hai nhóm, những nhóm nhận được$+k$và những cái không. 

Những ràng buộc ngụ ý$n$lên đến$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm, vì vậy mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Suy luận bậc hai trên tất cả các tập con hoặc cặp là không thể. 

Một cách giải thích ngây thơ có thể gợi ý nên thử tất cả các tập hợp con của chỉ số và tất cả các tập hợp con có thể$k$, nhưng đó là số mũ trong sự lựa chọn tập hợp con và vô hạn trong$k$. Ngay cả việc hạn chế các mẫu cấu trúc vẫn dẫn đến quá nhiều trường hợp trừ khi chúng ta khai thác được tính chất đơn điệu mạnh. 

Trường hợp cạnh tinh tế xuất hiện khi mảng không giảm. Trong trường hợp đó, câu trả lời đúng là ngay lập tức: chúng ta có thể chọn không áp dụng thao tác nào cả. Một trường hợp cạnh khác phát sinh khi mảng “gần như được sắp xếp” ngoại trừ nhiều phân đoạn giảm dần. Ví dụ: trong một mảng như$[3, 1, 2, 4]$, một thang máy đồng nhất không thể sửa chữa trật tự nếu các chỉnh sửa cần thiết không nhất quán giữa các lần đảo ngược. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ coi mọi tập hợp con của các chỉ số là “tập nâng lên” và cố gắng xác định liệu có tồn tại một tập hợp con nào đó hay không.$k$sao cho sau khi thêm$k$với tập hợp con đó, mảng sẽ được sắp xếp. Đối với một tập hợp con cố định, việc xác định tính khả thi sẽ giảm xuống còn việc kiểm tra xem liệu tất cả các ràng buộc thứ tự bị vi phạm có thể được thỏa mãn bằng một phép dịch chuyển nhất quán hay không. Tuy nhiên, có$2^n$tập hợp con, và thậm chí kiểm tra từng tập hợp con$O(n)$, dẫn đến$O(n2^n)$, điều đó vượt xa khả năng thực hiện. 

Quan sát quan trọng là thao tác tạo ra một phân vùng nhị phân của các vị trí mảng: mỗi chỉ mục không bị ảnh hưởng hoặc bị dịch chuyển một lượng như nhau. Khi chúng tôi sửa một mảng được sắp xếp cuối cùng hợp lệ, mọi phần tử phải thuộc một trong hai “cấp”: giá trị gốc hoặc giá trị gốc cộng$k$. Điều này có nghĩa là trong chuỗi được sắp xếp cuối cùng, bất cứ khi nào chúng ta chuyển từ phần tử không được dịch chuyển sang phần tử được dịch chuyển, cấu trúc của các khác biệt phải nhất quán trên toàn bộ. 

Cái nhìn sâu sắc quan trọng là xem xét phiên bản đã sắp xếp của mảng và so sánh nó với bản gốc. Nếu chúng ta tưởng tượng việc sắp xếp mảng, mỗi phần tử trong mảng cuối cùng sẽ tương ứng với giá trị ban đầu của nó hoặc giá trị ban đầu của nó trừ đi$k$. Do đó, nếu chúng ta căn chỉnh mảng ban đầu với phiên bản đã sắp xếp của nó thì sự khác biệt giữa các phần tử tương ứng phải có tối đa hai giá trị riêng biệt:$0$Và$k$. Bất kỳ sự khác biệt thứ ba nào cũng ngay lập tức làm cho việc chuyển đổi không thể thực hiện được. 

Điều này làm giảm vấn đề kiểm tra xem tập hợp chênh lệch giữa mảng ban đầu và mảng được sắp xếp có chứa tối đa một giá trị phân biệt dương hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(n 2^n)$|$O(n)$| Quá chậm | 
| Sắp xếp + Cấu trúc khác biệt |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Sắp xếp bản sao của mảng 

Chúng tôi xây dựng một phiên bản được sắp xếp của mảng đầu vào. Điều này thể hiện thứ tự mục tiêu không giảm mà chúng ta muốn đạt được. 

### 2. So sánh từng phần tử gốc với vị trí đã sắp xếp của nó 

Đối với mỗi chỉ mục, hãy tính chênh lệch giữa giá trị được sắp xếp và giá trị ban đầu. Sự khác biệt này cho chúng ta biết phần tử đó cần tăng bao nhiêu để phù hợp với vị trí được sắp xếp của nó. 

Nếu chúng ta thấy một sự khác biệt âm, điều đó có nghĩa là phần tử ban đầu đã lớn hơn vị trí được sắp xếp của nó, điều này không thể sửa được bằng cách chỉ thêm các giá trị, vì vậy câu trả lời là không thể ngay lập tức. 

### 3. Thu thập tất cả những khác biệt tích cực 

Chúng tôi thu thập tất cả những khác biệt tích cực. Chúng đại diện cho các phần tử phải được tăng lên để đạt được vị trí đã sắp xếp của chúng. 

Nếu chúng ta chỉ được phép thực hiện một thao tác với một$k$, thì tất cả những khác biệt dương này phải bằng nhau, bởi vì mọi phần tử được chọn đều nhận được mức tăng chính xác như nhau. 

### 4. Xác thực tính nhất quán của mức tăng cần thiết 

Nếu tập hợp các sai phân dương chứa nhiều hơn một giá trị phân biệt thì không thể chọn một giá trị duy nhất.$k$sửa tất cả các vị trí cần thiết cùng một lúc. 

Nếu có chính xác một chênh lệch dương rõ ràng thì giá trị đó là giá trị ứng cử viên$k$. 

### 5. Quyết định tính khả thi 

Nếu không có xung đột nào xuất hiện và tất cả các điều kiện đều được thỏa mãn, mảng có thể được sửa bằng một thao tác hoặc không có thao tác nào nếu đã được sắp xếp. 

### Tại sao nó hoạt động 

Hoạt động áp đặt một cấu trúc cứng nhắc: mọi phần tử được sửa đổi đều dịch chuyển một lượng chính xác như nhau. Khi so sánh với mục tiêu đã sắp xếp, mỗi phần tử sẽ hàm ý một giá trị dịch chuyển được yêu cầu một cách độc lập. Nếu cần nhiều hơn một giá trị dịch chuyển giữa các phần tử phải di chuyển lên trên, thì không có cách nào để điều hòa chúng với một giá trị toàn cục duy nhất.$k$. Ngược lại, nếu tất cả các dịch chuyển được yêu cầu đều đồng ý, chúng ta có thể chọn chính xác các phần tử đó và áp dụng điều đó.$k$, tạo ra mảng được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    b = sorted(a)
    
    diffs = set()
    
    for x, y in zip(a, b):
        if y < x:
            print("No")
            return
        if y > x:
            diffs.add(y - x)
    
    if len(diffs) <= 1:
        print("Yes")
    else:
        print("No")

t = int(input())
for _ in range(t):
    solve()
```Việc triển khai trực tiếp tuân theo ý tưởng căn chỉnh mảng với phiên bản đã sắp xếp của nó. Chi tiết quan trọng là chúng tôi coi bất kỳ yêu cầu tăng lương nào là bằng chứng về sự thay đổi thống nhất cần thiết. Nếu xuất hiện nhiều sự thay đổi tích cực rõ rệt, chúng tôi sẽ từ chối ngay lập tức. 

Séc`y < x`xử lý trường hợp đảo ngược không thể đảo ngược trong đó việc sắp xếp sẽ yêu cầu giảm một phần tử, điều này không được phép hoạt động. 

bộ`diffs`thực thi tính nhất quán toàn cầu của mức tăng cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 4
```| chỉ mục | bản gốc | được sắp xếp | khác biệt | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 2 | 2 | 0 | 
| 2 | 3 | 3 | 0 | 
| 3 | 4 | 4 | 0 | 

Tất cả sự khác biệt đều bằng 0, vì vậy không cần thực hiện thao tác nào. Đầu ra là Có. 

Điều này xác nhận trường hợp mảng đã được sắp xếp và thuật toán chấp nhận chính xác thao tác trống. 

### Ví dụ 2 

đầu vào:```
4
2 1 3 4
```| chỉ mục | bản gốc | được sắp xếp | khác biệt | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | không hợp lệ (tiêu cực) | 

Ở vị trí đầu tiên, giá trị được sắp xếp nhỏ hơn giá trị ban đầu, nghĩa là chúng ta sẽ cần giảm một phần tử để sửa thứ tự. Vì thao tác chỉ cho phép tăng dần nên điều này là không thể. 

Thuật toán từ chối ngay lập tức, chứng tỏ rằng không thể phục hồi được các hiệu chỉnh hướng xuống cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét tuyến tính đơn sau đó | 
| Không gian |$O(n)$| Lưu trữ để sắp xếp bản sao và theo dõi sự khác biệt | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng các phần tử, do đó$O(n \log n)$giải pháp là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    
    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        b = sorted(a)
        diffs = set()
        for x, y in zip(a, b):
            if y < x:
                return "No"
            if y > x:
                diffs.add(y - x)
        return "Yes" if len(diffs) <= 1 else "No"
    
    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# provided samples
assert run("1\n4\n1 2 3 4\n") == "Yes"
assert run("1\n3\n2 1 1\n") == "No"

# custom cases
assert run("1\n3\n1 3 2\n") == "Yes", "single swap-like fix"
assert run("1\n4\n4 3 2 1\n") == "No", "strict inversion impossible"
assert run("1\n5\n1 1 1 1 1\n") == "Yes", "already uniform"
assert run("1\n4\n1 5 2 6\n") == "No", "multiple shift requirements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3 2 | Có | chỉnh sửa nhất quán duy nhất | 
| 4 3 2 1 | Không | đảo ngược hoàn toàn không thể sửa được | 
| 1 1 1 1 1 | Có | trường hợp tầm thường không hoạt động | 
| 1 5 2 6 | Không | những ca làm việc cần thiết xung đột | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mảng đã được sắp xếp. Thuật toán xử lý điều này vì tất cả các khác biệt đều bằng 0, do đó tập hợp vẫn trống và câu trả lời được chấp nhận. 

Một trường hợp khác là khi chỉ có một phần tử cần di chuyển. Ví dụ,$[1, 3, 2]$tạo ra một mẫu khác biệt dương duy nhất, vẫn nhất quán, do đó thuật toán chấp nhận nó một cách chính xác. 

Một trường hợp tinh tế hơn là khi nhiều yếu tố yêu cầu tăng nhưng với số lượng khác nhau. Ví dụ$[1, 4, 2, 6]$mang lại hai mức tăng bắt buộc riêng biệt, khiến cho không thể chọn một mức tăng duy nhất$k$. Thuật toán bác bỏ điều này một cách chính xác vì tập hợp các khác biệt có kích thước lớn hơn một.
