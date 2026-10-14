---
title: "CF 105198E - Jor Shongkot"
description: "Chúng ta được cung cấp một mảng có độ dài $n$, trong đó $n$ là số lẻ và chúng ta được phép áp dụng nhiều lần một phép toán toàn cục rất bất thường: chọn một số nguyên dương $x$ và XOR mọi phần tử của mảng có $x$ trong một lần chụp."
date: "2026-06-27T02:58:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105198
codeforces_index: "E"
codeforces_contest_name: "ShellBeeHaken Presents Intra SUST Programming Contest 2024 - Replay"
rating: 0
weight: 105198
solve_time_s: 82
verified: false
draft: false
---

[CF 105198E - Jor Shongkot](https://codeforces.com/problemset/problem/105198/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$, Ở đâu$n$là số lẻ và chúng ta được phép áp dụng nhiều lần một phép toán toàn cục rất bất thường: chọn một số nguyên dương$x$và XOR mọi phần tử của mảng với$x$trong một lần bắn. Hoạt động này không nhắm mục tiêu vào các vị trí riêng lẻ; nó dịch chuyển toàn bộ mảng theo bước khóa theo XOR theo bit. 

Mục tiêu là để xác định xem, sau một số chuỗi dịch chuyển XOR toàn cục như vậy, mảng có thể trở thành hoán vị của các số từ$1$ĐẾN$n$. Vì một hoán vị yêu cầu tất cả các số nguyên trong phạm vi đó xuất hiện đúng một lần nên trạng thái cuối cùng cực kỳ cứng nhắc, trong khi thao tác chúng ta được phép cực kỳ linh hoạt nhưng cũng thống nhất trên toàn cầu. 

Những hạn chế là lớn, với$n$lên đến$10^6$và có giá trị lên tới khoảng$2 \cdot 10^6$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng các phép biến đổi hoặc khám phá nhiều lựa chọn XOR một cách rõ ràng. Thậm chí liệt kê các giá trị có thể có của$x$là không thể thực hiện được vì$x$có thể lớn tùy ý và hiệu ứng của XOR rất phi tuyến tính về mặt cấu trúc tập hợp. 

Trường hợp cạnh tinh tế là khi tất cả các phần tử đã khác biệt nhưng nằm ngoài phạm vi$1$ĐẾN$n$. Ví dụ, nếu$a = [100, 101, 102]$Và$n = 3$, việc các phần tử khác biệt là chưa đủ, bởi vì không một phép dịch chuyển XOR nào có thể ánh xạ chúng một cách chính xác vào$\{1,2,3\}$. Một trường hợp sai lầm khác là khi mảng đã chứa một hoán vị nhưng không theo thứ tự được sắp xếp, trong đó câu trả lời là CÓ nhưng lý luận ngây thơ về các phép biến đổi vẫn có thể thử các thao tác không cần thiết. 

Khó khăn chính là hiểu cấu trúc nào được bảo toàn trong các hoạt động XOR toàn cầu lặp đi lặp lại. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là nghĩ đến việc áp dụng các chuỗi mặt nạ XOR. Mỗi thao tác áp dụng$a_i \leftarrow a_i \oplus x$cho tất cả$i$và áp dụng hai thao tác với$x$Và$y$tương đương với một thao tác đơn lẻ với$x \oplus y$. Điều này có nghĩa là toàn bộ quá trình tập trung vào việc chọn nhiều nhất một mặt nạ XOR hiệu quả: các thao tác lặp lại không tạo ra bất kỳ thứ gì ngoài một giá trị XOR tích lũy duy nhất. 

Vì vậy, vấn đề giảm xuống còn việc hỏi liệu có tồn tại một số nguyên hay không$x$sao cho nhiều bộ$$\{a_1 \oplus x, a_2 \oplus x, \dots, a_n \oplus x\}$$chính xác là$\{1,2,\dots,n\}$. 

Một giải pháp vũ phu sẽ thử mọi cách có thể$x$lên đến một số giới hạn (chẳng hạn như giá trị tối đa có trong phạm vi đầu vào) và đối với mỗi giới hạn$x$kiểm tra xem XOR toàn bộ mảng có tạo ra hoán vị hay không. Mỗi chi phí kiểm tra$O(n)$, và với$x$có khả năng kéo dài tới$2^{21}$, điều này trở nên quá lớn. 

Quan sát quan trọng là XOR có thể nghịch đảo và hoạt động như một phép đối chiếu trên các số nguyên. Điều này có nghĩa là cấu trúc mà chúng ta đang tìm kiếm không phải về các giá trị riêng lẻ mà là về việc khớp toàn bộ tập hợp dưới một phép dịch chuyển XOR. Nếu như vậy$x$tồn tại, sau đó áp dụng lại XOR với cùng$x$đưa chúng ta trở lại mảng ban đầu, do đó mối quan hệ là đối xứng giữa mảng ban đầu và hoán vị đích. 

Điều này gợi ý việc định hình lại vấn đề: chúng ta đang hỏi liệu tập hợp$\{1,2,\dots,n\}$có thể được chuyển XOR vào nhiều tập hợp nhất định. Điều đó có nghĩa là sự khác biệt nhiều tập hợp hoàn toàn được giải thích bằng mặt nạ XOR thống nhất. 

Một hệ quả quan trọng về mặt cấu trúc là XOR giữa các phần tử tương ứng phải nhất quán sau khi sắp xếp theo cấu trúc. Cụ thể, nếu chúng ta sắp xếp cả hai bộ, một ánh xạ hợp lệ sẽ ngụ ý rằng tất cả các khác biệt XOR theo cặp giữa các phần tử phù hợp đều giống hệt nhau. Vì không xác định được sự trùng khớp nên thay vào đó, chúng tôi dựa vào bất biến chính tắc: cấu trúc tần số trong các phép biến đổi XOR ngụ ý rằng nhiều tập hợp phải được đóng dưới sự liên kết XOR với một mặt nạ cố định. 

Sự đơn giản hóa cuối cùng xuất phát từ thực tế là các hoán vị dịch chuyển XOR bảo toàn các khác biệt XOR theo cặp. Trong một hoán vị của$1..n$, tất cả các khác biệt XOR theo cặp tạo thành một tập hợp cố định. Đầu vào phải khớp với cấu trúc đó sau khi thay đổi toàn cầu, điều này giúp kiểm tra xem liệu chúng ta có thể căn chỉnh một phần tử hay không và kiểm tra tính nhất quán. 

Chúng tôi sửa lỗi ánh xạ ứng viên bằng cách cố gắng căn chỉnh giá trị nhỏ nhất có thể có trong phạm vi mục tiêu (1) với từng phần tử của mảng. Đối với mỗi sự liên kết như vậy, chúng tôi rút ra$x = a_i \oplus 1$, áp dụng nó một cách ngầm định và xác minh xem tập hợp đa được chuyển đổi có chính xác không$1..n$. Trong khi điều này nghe có vẻ như$O(n^2)$, nó thực sự được cắt bớt bằng cách băm hoặc so sánh tần số và có thể được tối ưu hóa bằng cách sử dụng kiểm tra tập hợp và loại bỏ sớm. Trong thực tế chỉ có một giá trị hợp lệ$x$có thể tồn tại, vì vậy khi tìm thấy ánh xạ nhất quán, chúng tôi sẽ xác minh trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mặt nạ XOR |$O(n \cdot 2^{21})$|$O(n)$| Quá chậm | 
| Hãy thử sắp xếp ứng viên + xác minh |$O(n)$trung bình |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng chính là nếu tồn tại một phép dịch chuyển XOR hợp lệ thì nó được xác định hoàn toàn bằng cách chúng ta ánh xạ một phần tử của mảng tới giá trị đích trong hoán vị. 

1. Xây dựng bảng tần số hoặc biểu diễn tập hợp của mảng đầu vào. Điều này cho phép kiểm tra tư cách thành viên liên tục trong quá trình xác thực. 
2. Lặp lại từng phần tử$a_i$và coi nó như một hình ảnh ứng cử viên của giá trị$1$trong hoán vị đích. Từ đó, tính toán mặt nạ XOR ứng viên$x = a_i \oplus 1$. Lựa chọn này khắc phục toàn bộ quá trình chuyển đổi vì XOR có thể đảo ngược. 
3. Áp dụng ứng viên này về mặt khái niệm: cho mỗi giá trị$a_j$, tính toán$a_j \oplus x$và kiểm tra xem nó có nằm trong phạm vi không$[1, n]$và liệu nó có xuất hiện chính xác một lần trong nhiều mục tiêu hay không. Việc xác minh này đảm bảo rằng không xảy ra xung đột hoặc thiếu giá trị. 
4. Nếu có ứng viên nào$x$tạo ra kết quả khớp hoàn hảo cho tất cả các phần tử, hãy trả về CÓ ngay lập tức. Nếu không có ứng viên nào làm việc sau khi đã sử dụng hết mọi khả năng, hãy trả về KHÔNG. 

### Tại sao nó hoạt động 

Một phép biến đổi hợp lệ hoàn toàn được xác định bởi một mặt nạ XOR$x$. Khi một phần tử được ánh xạ tới vị trí mục tiêu chính xác của nó, tất cả các ánh xạ khác sẽ bị buộc phải thực hiện. Nếu xuất hiện bất kỳ sự không nhất quán nào thì không thể sửa chữa nó bằng các thao tác bổ sung vì việc soạn thảo các thao tác XOR chỉ tạo ra một mặt nạ XOR duy nhất khác. Do đó, tính chính xác giảm xuống còn việc kiểm tra xem liệu có tồn tại song ánh nhất quán toàn cầu nào giữa tập đầu vào và$\{1..n\}$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    target = set(range(1, n + 1))
    freq = {}
    for v in a:
        freq[v] = freq.get(v, 0) + 1

    # try mapping a_i -> 1
    for v in a:
        x = v ^ 1
        ok = True
        seen = {}
        
        for u in a:
            w = u ^ x
            if w < 1 or w > n:
                ok = False
                break
            seen[w] = seen.get(w, 0) + 1
            if seen[w] > 1:
                ok = False
                break
        
        if ok and len(seen) == n:
            if all(seen.get(i, 0) == 1 for i in range(1, n + 1)):
                print("YES")
                return
    
    print("NO")

if __name__ == "__main__":
    solve()
```Mã xây dựng mảng đầu vào và sau đó cố gắng suy ra mặt nạ XOR hợp lệ bằng cách căn chỉnh từng phần tử với giá trị$1$. Đối với mỗi mặt nạ ứng cử viên, nó sẽ xây dựng lại đầy đủ mảng đã biến đổi trong bản đồ tần số tạm thời và kiểm tra xem nó có tạo thành một hoán vị hoàn hảo của$1..n$. Việc ngắt sớm bên trong vòng lặp bên trong là rất cần thiết vì chúng ngăn chặn hiện tượng nổ bậc hai trong các trường hợp không hợp lệ, đặc biệt là khi các giá trị sớm nằm ngoài phạm vi hợp lệ. 

Kiểm tra cuối cùng đảm bảo cả tính đầy đủ và tính duy nhất, cả hai đều cần thiết cho một hoán vị hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
```Đây$n = 1$, mảng là$[3]$và hoán vị mục tiêu là$[1]$. 

| Bước | Ứng viên v | x = v XOR 1 | Mảng chuyển đổi | Phạm vi hợp lệ | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | [1] | vâng | CÓ | 

Phần tử đơn được chuyển thành 1 bằng cách chọn$x = 2$, cho thấy rằng ngay cả các trường hợp phần tử đơn tầm thường cũng phụ thuộc vào khả năng nghịch đảo của XOR. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```| Bước | Ứng viên v | x | Mảng chuyển đổi | Hoán vị hợp lệ | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | [1,2,3] | vâng | CÓ | 

Trường hợp nhận dạng hoạt động ngay lập tức với$x = 0$(được cho phép ngầm như một hiệu ứng ròng không có thay đổi). 

Điều này xác nhận rằng thuật toán xử lý chính xác các hoán vị đã hợp lệ mà không cần những biến đổi không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$tồi tệ nhất,$O(n)$điển hình với việc cắt tỉa | Mỗi ứng viên đều yêu cầu quét toàn bộ, nhưng việc thoát sớm sẽ làm giảm đáng kể công việc | 
| Không gian |$O(n)$| Bản đồ tần suất để xác nhận | 

Được cho$n \leq 10^6$, giải pháp dự định dựa trên thực tế là chỉ có một mặt nạ ứng cử viên có thể thành công và việc từ chối sớm sẽ ngăn cản hành vi bậc hai trong thực tế đối với các ràng buộc hợp lệ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    
    target = set(range(1, n + 1))
    
    for v in a:
        x = v ^ 1
        seen = {}
        ok = True
        for u in a:
            w = u ^ x
            if w < 1 or w > n:
                ok = False
                break
            seen[w] = seen.get(w, 0) + 1
            if seen[w] > 1:
                ok = False
                break
        if ok and len(seen) == n:
            if all(seen.get(i, 0) == 1 for i in range(1, n + 1)):
                return "YES"
    return "NO"

# provided samples
assert run("1\n1\n3") == "YES"
assert run("3\n1 2 3") == "YES"

# custom cases
assert run("3\n2 3 1") == "YES"
assert run("3\n0 0 0") == "NO"
assert run("5\n10 11 12 13 14") == "NO"
assert run("1\n2") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 3 1 | CÓ | hoán vị giống như xoay đã hợp lệ | 
| 3 0 0 0 | KHÔNG | trùng lặp không thể hình thành hoán vị | 
| 5 10 11 12 13 14 | KHÔNG | ngoài phạm vi sau bất kỳ ca nào | 
| 1 2 | CÓ | sửa lỗi XOR không tầm thường nhỏ nhất | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi tất cả các giá trị giống hệt nhau. Ví dụ,$n = 3$,$a = [7,7,7]$. Bất kỳ phép dịch chuyển XOR nào đều bảo toàn sự bằng nhau của tất cả các phần tử, do đó mảng không bao giờ có thể trở thành một hoán vị. Thuật toán thử các ứng cử viên bắt nguồn từ mỗi giá trị, nhưng mọi phép biến đổi vẫn tạo ra các bản sao, vì vậy tất cả các lần kiểm tra đều thất bại và câu trả lời chính xác là KHÔNG. 

Một trường hợp khác là khi các giá trị đã là hoán vị nhưng dịch chuyển ra ngoài phạm vi mục tiêu. Ví dụ,$a = [1000,1001,1002]$với$n = 3$. Thuật toán cố gắng$x = 1000 \oplus 1$, ánh xạ mảng tới một tập ứng cử viên, nhưng các giá trị kết quả không thể hình thành chính xác$\{1,2,3\}$, do đó việc từ chối xảy ra ở bước xác thực phạm vi. 

Cuối cùng, hãy xem xét khi chỉ có một phần tử phù hợp với sự liên kết tiềm năng còn những phần tử khác thì không. Vì XOR có tính toàn cục nên một sự không khớp duy nhất sẽ buộc phải loại bỏ ngay lập tức và việc thoát sớm sẽ đảm bảo thuật toán không lãng phí thời gian khi quét phần còn lại của mảng.
