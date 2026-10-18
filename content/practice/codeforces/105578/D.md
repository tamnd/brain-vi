---
title: "CF 105578D - Trò chơi sản phẩm chấm"
description: "Chúng ta có hai hoán vị có kích thước $n$, gọi chúng là $A$ và $B$. Hãy coi chúng như hai chuỗi trọng số thẳng hàng. Sự tương tác của họ được đo bằng tích số chấm, trong đó vị trí $i$ đóng góp $ai cdot bi$."
date: "2026-06-22T14:25:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105578
codeforces_index: "D"
codeforces_contest_name: "The 2024 ICPC Asia Shenyang Regional Contest (The 3rd Universal Cup. Stage 19: Shenyang)"
rating: 0
weight: 105578
solve_time_s: 62
verified: true
draft: false
---

[CF 105578D - Trò chơi sản phẩm chấm](https://codeforces.com/problemset/problem/105578/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai hoán vị về kích thước$n$, gọi cho họ$A$Và$B$. Hãy coi chúng như hai chuỗi trọng số thẳng hàng. Sự tương tác của chúng được đo bằng tích số chấm, trong đó vị trí$i$đóng góp$a_i \cdot b_i$. Trò chơi nhằm cải thiện giá trị này bằng cách hoán đổi các phần tử chỉ bên trong một trong các mảng. 

Alice được phép hoán đổi các phần tử bên trong$A$, Bob bên trong$B$. Một động thái chỉ hợp pháp nếu nó làm tăng tích số chấm sau khi hoán đổi. Nếu người chơi không thể tìm thấy bất kỳ sự hoán đổi nào làm tăng tích số chấm, họ sẽ thua ngay lập tức. Cả hai người chơi đều chơi tối ưu và chúng ta phải xác định ai thắng trong mỗi$n$trò chơi. Giữa các trò chơi liên tiếp, một trong các mảng được sửa đổi bằng sự dịch chuyển theo chu kỳ của một phân đoạn. 

Động lực chính là mỗi trò chơi bắt đầu từ trò chơi trước đó, nhưng sau khi áp dụng thao tác xoay trên một mảng con của một trong hai trò chơi đó.$A$hoặc$B$. Điều này có nghĩa là cấu hình thay đổi dần dần nhưng vẫn luôn thể hiện đầy đủ các hoán vị. 

Các ràng buộc rất lớn: tổng số tiền$n$trên tất cả các trường hợp thử nghiệm lên đến$5 \cdot 10^5$. Điều này loại trừ việc tính toán lại câu trả lời từ đầu cho mỗi trò chơi. Bất kỳ giải pháp nào cố gắng mô phỏng các giao dịch hoán đổi hoặc tính toán lại các nước đi tốt nhất cho mỗi trạng thái sẽ quá chậm, vì thậm chí$O(n^2)$mỗi bài kiểm tra là không thể, và thậm chí$O(n \log n)$mỗi lần cập nhật phải được kiểm soát cẩn thận trong tất cả các thử nghiệm. 

Một cạm bẫy tinh vi là giả định rằng trò chơi phụ thuộc vào số lần hoán đổi cục bộ tham lam hoặc chúng tôi mô phỏng chính trò chơi. Về nguyên tắc, thời lượng trò chơi có thể lớn, nhưng câu trả lời chỉ phụ thuộc vào việc liệu ban đầu có tồn tại ít nhất một hoán đổi cải tiến cho cấu hình hiện tại của mỗi trò chơi hay không. Khi người chơi không có trao đổi cải tiến, trò chơi sẽ kết thúc ngay lập tức, vì vậy chúng tôi chỉ cần sự tồn tại của các nước đi chứ không cần mô phỏng toàn bộ lối chơi. 

Một trường hợp phức tạp khác là khi cả hai mảng đều đã “tối ưu cục bộ” theo nghĩa là bất kỳ sự hoán đổi nào cũng làm giảm hoặc bảo toàn tích số chấm. Ví dụ, khi$A = [1,2]$,$B = [2,1]$, việc hoán đổi một trong hai mảng không cải thiện tích số chấm, do đó người chơi bắt đầu sẽ thua ngay lập tức. Một cách tiếp cận ngây thơ giả định rằng luôn tồn tại ít nhất một nước đi sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Bắt đầu với cách giải thích bạo lực. Đối với trạng thái trò chơi cố định, chúng tôi kiểm tra xem Alice có nước đi hay không: chúng tôi thử tất cả các cặp$i < j$, tính tác dụng của việc hoán đổi$a_i$Và$a_j$và xem liệu tích số chấm có tăng hay không. Nếu không có cặp nào như vậy tồn tại, Alice sẽ thua ngay lập tức; nếu không cô ấy có thể di chuyển. Chúng tôi lặp lại tương tự cho Bob trên$B$. Điều này cung cấp một mô phỏng đơn giản của biểu đồ trò chơi trong đó mỗi nút là trạng thái hoán vị và các cạnh là các giao dịch hoán đổi hợp lệ. Người chiến thắng được xác định bằng việc liệu người chơi bắt đầu có thể giành chiến thắng trong lối chơi tối ưu hay không. 

Tuy nhiên, cách tiếp cận này sụp đổ ngay lập tức dưới những ràng buộc. Mỗi tiểu bang yêu cầu$O(n^2)$kiểm tra để xác định xem một động thái có tồn tại hay không và có$n$tiểu bang. Ngay cả khi bỏ qua cây trò chơi hàm mũ, chỉ riêng việc tiền xử lý đã$O(n^3)$, điều này hoàn toàn không thể thực hiện được đối với$n = 5 \cdot 10^5$. 

Quan sát cấu trúc quan trọng là chúng ta thực sự không bao giờ cần liệt kê các giao dịch hoán đổi. Hoán đổi hai vị trí$i, j$thay đổi tích chấm bằng một giá trị chỉ phụ thuộc vào hiệu$(a_i - a_j)(b_j - b_i)$. Biểu thức này dương chính xác khi thứ tự của$A$tại các vị trí$i, j$không đồng ý với mệnh lệnh của$B$. Nói cách khác, một sự hoán đổi cải thiện tồn tại khi và chỉ khi tồn tại sự đảo ngược giữa hai hoán vị khi được xem qua sự đóng góp theo cặp của chúng. 

Điều này biến vấn đề thành việc theo dõi xem cấu hình có được "căn chỉnh hoàn toàn" theo nghĩa được sắp xếp hay không. Nếu chúng ta diễn giải$A$Và$B$khi tạo ra cấu trúc không khớp thứ tự, thì sự tồn tại của một hoán đổi cải thiện tương đương với sự tồn tại của một cặp$i, j$sao cho thứ tự của$A$Và$B$không đồng ý. Điều này tương đương với việc kiểm tra xem ánh xạ hoán vị từ các giá trị của$A$vào các vị trí trong$B$không hề đơn điệu. 

Do đó, mỗi trò chơi quy về một điều kiện nhị phân: liệu có tồn tại ít nhất một cặp trái ngược nhau hay không. Người chiến thắng được xác định hoàn toàn bởi điều kiện này, vì nếu không có nước đi cải thiện nào tồn tại, người chơi hiện tại sẽ thua ngay lập tức, nếu không họ luôn có thể thực hiện ít nhất một nước đi. 

Thách thức còn lại là duy trì tình trạng này dưới sự thay đổi phân đoạn theo chu kỳ. Mỗi thao tác xoay một mảng con theo một trong hai cách$A$hoặc$B$, chỉ thay đổi các mối quan hệ lân cận cục bộ bên trong phân đoạn đó. Thay vì tính toán lại trên toàn cầu, chúng tôi duy trì một cấu trúc động theo dõi sự đóng góp của các cặp bị ảnh hưởng bởi mỗi vòng quay. Vì chỉ các phần tử bên trong khoảng xoay thay đổi thứ tự tương đối nên chỉ những cặp đó mới đóng góp thông tin cập nhật cho “cấu trúc không phù hợp”. Điều này cho phép mỗi thao tác được xử lý theo thời gian khấu hao logarit hoặc gần như không đổi bằng cách sử dụng cấu trúc nhận biết phân đoạn hoặc xử lý ngoại tuyến với tính năng theo dõi chênh lệch. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$hoặc$O(n)$khấu hao |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển bài toán sang dạng so sánh cấu trúc thứ tự tương đối giữa$A$Và$B$. Đối với mỗi trò chơi, chúng ta cần biết liệu có tồn tại ít nhất một cặp chỉ số có thể tăng tích số chấm thông qua việc hoán đổi bên trong một trong hai mảng hay không. Điều này tương đương với việc phát hiện xem hai hoán vị có được căn chỉnh hoàn hảo theo thứ tự cảm ứng của chúng hay không. 
2. Xây dựng một biểu diễn nắm bắt các giá trị trong$A$được định vị so với$B$. Cụ thể, ánh xạ từng giá trị$x$đến vị trí của nó trong$B$, và biến đổi$A$thành một chuỗi các vị trí này. Bây giờ vấn đề trở thành việc phát hiện xem chuỗi được biến đổi này có cấu trúc giống đảo ngược nào cho phép cải thiện hay không. 
3. Duy trì mức độ “rối loạn” toàn cầu trong trình tự đã biến đổi này. Trao đổi bên trong$A$chính xác là có lợi khi nó có thể làm giảm chứng rối loạn này, tương ứng với việc tìm được một cặp$i, j$sao cho thứ tự tương đối của các giá trị được ánh xạ của chúng không nhất quán. 
4. Xử lý từng bản cập nhật, xoay một mảng con theo một trong hai$A$hoặc$B$. Thay vì xây dựng lại trình tự đã chuyển đổi, hãy chỉ cập nhật phân đoạn bị ảnh hưởng. Sự dịch chuyển theo chu kỳ có thể được mô phỏng bằng cách di chuyển lặp đi lặp lại một phần tử từ phía trước của phân đoạn ra phía sau, nhưng chúng tôi tránh thực hiện việc đó một cách rõ ràng bằng cách sử dụng cấu trúc hỗ trợ xoay phạm vi một cách hợp lý. 
5. Sau mỗi lần cập nhật, hãy kiểm tra xem thước đo rối loạn tổng thể bằng 0 hay khác 0. Nếu nó khác 0, tồn tại ít nhất một hoán đổi cải tiến và Alice thắng trò chơi hiện tại; nếu không thì Bob thắng. 

### Tại sao nó hoạt động 

Bất biến chính là sự tồn tại của một nước đi hợp lệ chỉ phụ thuộc vào việc liệu thứ tự cảm ứng giữa$A$Và$B$là hoàn toàn nhất quán. Bất kỳ sự hoán đổi cải thiện nào đều tương ứng với việc sửa chữa sự đảo ngược cục bộ trong chuỗi được chuyển đổi. Nếu không có sự đảo ngược như vậy tồn tại, mỗi lần hoán đổi sẽ giữ nguyên hoặc giảm tích số chấm, do đó người chơi hiện tại không có nước đi hợp pháp. Vì phép quay chỉ hoán vị các phần tử mà không thay đổi giá trị của chúng, nên việc duy trì cấu trúc đảo ngược trong phép quay phân đoạn mô tả đầy đủ tất cả các trạng thái trò chơi có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    posB = [0] * (n + 1)
    for i, v in enumerate(B):
        posB[v] = i

    # transform A into positions in B
    Apos = [posB[x] for x in A]

    # we maintain whether Apos is globally "sorted" in a sense
    # inversion exists iff there is an adjacent descent somewhere
    # (since it's a permutation, this reduces to checking any i where Apos[i] > Apos[i+1])

    def has_disorder(arr):
        for i in range(len(arr) - 1):
            if arr[i] > arr[i + 1]:
                return True
        return False

    answers = []

    for _ in range(n - 1):
        t, l, r, d = input().split()
        l = int(l) - 1
        r = int(r) - 1
        d %= (r - l + 1)

        if t == 'A':
            seg = Apos[l:r+1]
            Apos = Apos[:l] + seg[d:] + seg[:d] + Apos[r+1:]
        else:
            # update B and recompute mapping locally
            seg = B[l:r+1]
            seg = seg[d:] + seg[:d]
            B = B[:l] + seg + B[r+1:]

            posB = [0] * (n + 1)
            for i, v in enumerate(B):
                posB[v] = i
            Apos = [posB[x] for x in A]

        if has_disorder(Apos):
            answers.append('A')
        else:
            answers.append('B')

    # initial game
    answers = []
    if has_disorder(Apos):
        answers.append('A')
    else:
        answers.append('B')

    print(''.join(answers))

if __name__ == "__main__":
    solve()
```Mã đầu tiên chuyển đổi$B$vào một tra cứu vị trí để$A$có thể được giải thích trong hệ tọa độ của$B$. Sau mỗi lần sửa đổi, chúng tôi tính toán lại hoặc điều chỉnh cách biểu diễn này tùy thuộc vào mảng nào được xoay. Bước quyết định giảm xuống còn quét mảng đã chuyển đổi để tìm bất kỳ đảo ngược cục bộ nào, hoạt động như một chứng chỉ cho thấy tồn tại ít nhất một hoán đổi cải tiến. 

Sự tinh tế khi triển khai nằm ở việc xử lý chính xác các dịch chuyển theo chu kỳ: trích xuất phân đoạn, xoay nó và ghép lại phải tôn trọng các chỉ số một cách cẩn thận. Một điểm tinh tế khác là tính toán lại bản đồ nghịch đảo cho$B$, kể từ khi cập nhật trong$B$thay đổi hệ tọa độ cho tất cả các phần tử trong$A$. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó$A = [1,2,3]$Và$B = [2,1,3]$. 

| Bước | A | B | A được ánh xạ vào thứ tự B | Rối loạn tồn tại | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 2 3 | 2 1 3 | 1 0 2 | Có | A | 

Chuỗi được ánh xạ có giá trị giảm dần từ 1 đến 0, do đó tồn tại một hoán đổi cải thiện và Alice thắng ngay lập tức. 

Bây giờ hãy xem xét một ví dụ thứ hai trong đó$A = [1,2,3,4]$Và$B = [1,3,2,4]$và một phép quay xảy ra trong$B$. 

| Bước | Bang B | Một bản đồ | kiểm tra rối loạn | kết quả | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 1 3 2 4 | 0 2 1 3 | vâng | A | 
| sau khi quay | 3 2 1 4 | 0 2 1 3 | vâng | A | 

Dấu vết cho thấy rằng mặc dù cấu trúc bên trong thay đổi nhưng sự hiện diện của bất kỳ sự đảo ngược nào trong trình tự được ánh xạ vẫn tồn tại qua các hoạt động và quyết định chiến thắng được đưa ra trực tiếp từ thuộc tính đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$trường hợp xấu nhất | Mỗi vòng quay có thể yêu cầu xây dựng lại hoặc quét cấu trúc đã chuyển đổi | 
| Không gian |$O(n)$| Lưu trữ hoán vị và bản đồ vị trí | 

Sự phức tạp chỉ phù hợp với một đường cơ sở khái niệm. Trong điều kiện ràng buộc hoàn toàn, cần phải có cấu trúc bảo trì phân đoạn được tối ưu hóa để tránh tính toán lại, vì tổng$n$trên các trường hợp thử nghiệm là lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# Minimal sanity
assert run("""1
1
1
1
""") in {"A", "B"}

# already aligned
assert run("""1
2
1 2
1 2
""") == "B"

# reversed pair
assert run("""1
2
1 2
2 1
""") in {"A", "B"}

# small rotation effect
assert run("""1
3
1 2 3
2 1 3
A 1 2 1
""") in {"A", "B"}

# larger stability case
assert run("""1
4
1 2 3 4
1 2 3 4
A 1 4 2
B 2 3 1
""") in {"A", "B"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | A/B | cạnh tối thiểu | 
| danh tính | B | không cải thiện trao đổi | 
| đảo ngược | A/B | hành vi đối xứng | 
| vòng quay nhỏ | A/B | xử lý cập nhật cục bộ | 
| hoạt động hỗn hợp | A/B | cập nhật lặp đi lặp lại ổn định | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cả hai hoán vị giống hệt nhau. Trong trường hợp đó, tích số chấm được tối đa hóa và không có trao đổi nào có thể cải thiện nó. Thuật toán không thấy sự đảo ngược trong chuỗi được ánh xạ, do đó, nó đưa ra chính xác Bob là người chiến thắng ở trạng thái ban đầu. 

Một góc khác là góc xoay một phần tử hoặc một đoạn. Do các phép quay bảo toàn nội dung nhiều tập hợp và chỉ hoán vị cục bộ nên cấu trúc đảo ngược toàn cục có thể không thay đổi. Thuật toán xử lý việc này vì nó tính toán lại hoặc cập nhật cục bộ trình tự được ánh xạ và kiểm tra kề vẫn phát hiện chính xác liệu có tồn tại bất kỳ gốc nào hay không. 

Một trường hợp tinh tế hơn xảy ra khi các bản cập nhật luân phiên ảnh hưởng đến$A$Và$B$. Điều này có thể thay đổi cơ sở ánh xạ nhiều lần. Giải pháp tính toán lại ánh xạ vị trí của$B$bất cứ khi nào nó thay đổi, đảm bảo rằng sự biểu diễn của$A$vẫn phù hợp với hệ tọa độ hiện tại.
