---
title: "CF 104521C - Sắp xếp lại gấu trúc đỏ"
description: "Chúng ta được cấp một bộ gấu trúc đỏ, nhưng thay vì biết vị trí của chúng trên hàng, chúng ta chỉ được biết mỗi cặp gấu trúc cách nhau bao xa. Riêng biệt, chúng ta còn được cung cấp một danh sách đã sắp xếp các tọa độ thực tế trên trục số."
date: "2026-06-30T10:19:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104521
codeforces_index: "C"
codeforces_contest_name: "CerealCodes II Novice"
rating: 0
weight: 104521
solve_time_s: 99
verified: false
draft: false
---

[CF 104521C - Sắp xếp lại gấu trúc đỏ](https://codeforces.com/problemset/problem/104521/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ gấu trúc đỏ, nhưng thay vì biết vị trí của chúng trên hàng, chúng ta chỉ được biết mỗi cặp gấu trúc cách nhau bao xa. Riêng biệt, chúng ta còn được cung cấp một danh sách đã sắp xếp các tọa độ thực tế trên trục số. Nhiệm vụ là quyết định xem gấu trúc nào sẽ đi đến tọa độ nào sao cho tất cả khoảng cách theo cặp khớp với sự khác biệt tuyệt đối giữa các tọa độ được chỉ định. 

Nói cách khác, tồn tại một hoán vị chưa biết của gấu trúc trên các vị trí đã cho sao cho với mỗi cặp gấu trúc, khoảng cách trong ma trận đầu vào bằng khoảng cách hình học giữa các điểm được chỉ định của chúng. Chúng tôi phải khôi phục mọi nhiệm vụ hợp lệ. 

Các ràng buộc đủ chặt chẽ để có thể chấp nhận được việc tái cấu trúc bậc hai cho mỗi trường hợp thử nghiệm, nhưng mọi thứ khối hoặc liên quan đến tính toán lại nặng nề lặp đi lặp lại trên mỗi bộ ba nút sẽ thất bại. Tổng của tất cả n nhiều nhất là 1000, điều này ngay lập tức cho thấy rằng O(n^2) cho mỗi trường hợp thử nghiệm là an toàn, trong khi O(n^3) cũng là giới hạn về mặt kỹ thuật có thể chấp nhận được nhưng không cần thiết và có rủi ro trong Python. 

Một vấn đề tế nhị nảy sinh từ tính đối xứng. Nếu cấu hình hợp lệ, việc đảo ngược toàn bộ thứ tự dọc theo dòng sẽ tạo ra một giải pháp hợp lệ khác. Điều đó có nghĩa là không có câu trả lời duy nhất và bất kỳ sự tái thiết nhất quán nào cũng đủ. Điều tinh tế thứ hai là chỉ riêng khoảng cách không trực tiếp tiết lộ tọa độ mà chỉ thể hiện thứ tự tương đối. 

Một sai lầm ngây thơ là cho rằng việc sắp xếp gấu trúc theo khoảng cách đến gấu trúc 1 sẽ tạo ra thứ tự đúng. Điều này không thành công bất cứ khi nào panda 1 không phải là điểm cuối của đoạn đường. 

Ví dụ: giả sử ba con gấu trúc nằm ở tọa độ 0, 5 và 10, nhưng gấu trúc 1 lại ở tọa độ 5. Khi đó khoảng cách từ gấu trúc 1 đều bằng 5 nên không thể phân biệt được bên nào là trái hay phải. Một kiểu sắp xếp ngây thơ sẽ làm sụp đổ cấu trúc một cách không chính xác hoặc phá vỡ các mối ràng buộc một cách tùy tiện, vi phạm các khoảng cách theo cặp khác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng gán gấu trúc vào các vị trí bằng cách thử tất cả các hoán vị và kiểm tra xem ma trận khoảng cách có khớp với các khác biệt tuyệt đối được tạo ra hay không. Điều này đơn giản về mặt khái niệm: đối với mỗi hoán vị, hãy xác minh tất cả các khoảng cách theo cặp. Chỉ riêng việc xác minh đã tốn O(n^2) và có n! hoán vị, điều này làm cho phương pháp này hoàn toàn không khả thi ngay cả khi n = 10. 

Thông tin chi tiết về cấu trúc quan trọng là các điểm trên không gian số liệu đường được xác định hoàn toàn theo sự phản ánh và dịch chuyển khi chúng tôi xác định được hai điểm cuối. Nếu chúng ta chọn một điểm cuối A thì điểm xa nhất so với điểm đó phải là điểm cuối đối diện B. Khi hai điểm neo này được cố định, mọi điểm còn lại đều có tọa độ duy nhất dọc theo trục đó, có thể tính toán hoàn toàn từ khoảng cách. 

Lý do điều này có tác dụng là vì trong một thước đo đường thực, đường dẫn giữa các điểm cuối là duy nhất và mọi điểm khác đều nằm trên đường đó. Các mối quan hệ khoảng cách thực thi một phép chiếu nhất quán lên đường đó. 

Điều này làm giảm vấn đề từ phép gán tổ hợp sang tái cấu trúc tọa độ xác định, sau đó là sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n^2) | O(n) | Quá chậm | 
| Tái thiết điểm cuối | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế là các điểm nằm chính xác trên một thước đo đường. 

## Hướng dẫn thuật toán 

1. Chọn một con gấu trúc tùy ý, để thuận tiện cho con gấu trúc 0, và tìm con gấu trúc ở xa nó nhất bằng cách sử dụng ma trận khoảng cách. Gọi gấu trúc này là A. Điểm xa nhất so với một điểm tùy ý trong thước đo đường phải là một trong các điểm cuối, vì điểm cuối tối đa hóa độ lệch tâm. 
2. Từ A, quét lại tất cả gấu trúc và tìm con có khoảng cách tối đa đến A. Gọi nó là B. Điều này đảm bảo A và B là hai điểm cuối của đường thẳng. 
3. Coi A và B như xác định trục. Với mỗi gấu trúc i, hãy tính tọa độ của nó bằng công thức

x[i] = (d[A][i] - d[B][i] + d[A][B]) / 2. 

Biểu thức này xuất phát từ việc giải hệ phương trình |x[i] - x[A]| và |x[i] - x[B]| với giả định rằng A và B là hai điểm cực trị của một đường thẳng. 
4. Sắp xếp gấu trúc theo tọa độ tính toán của chúng. Điều này khôi phục thứ tự từ trái sang phải của chúng trên dòng, cho đến độ ổn định về số. 
5. Gán các gấu trúc đã được sắp xếp vào các vị trí đã sắp xếp p1 < p2 < ... < pn theo thứ tự. Xuất ra các chỉ số tương ứng. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào thực tế là trong không gian mêtric 1D, bất kỳ điểm i nào cũng nằm trên đoạn đường duy nhất giữa các điểm cuối A và B, do đó vị trí của nó được xác định hoàn toàn bởi khoảng cách của nó đến A và B. Công thức tọa độ dẫn xuất tương đương với việc chiếu i lên trục được xác định bởi A và B. Khi tọa độ nhất quán với tất cả các khoảng cách theo cặp, việc sắp xếp chúng phải khớp với thứ tự hình học thực sự, bởi vì các vị trí thực p đã được sắp xếp dọc theo cùng trục đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        d = [list(map(int, input().split())) for _ in range(n)]

        if n == 1:
            print(1)
            continue

        A = 0
        for i in range(n):
            if d[0][i] > d[0][A]:
                A = i

        B = A
        for i in range(n):
            if d[A][i] > d[A][B]:
                B = i

        dab = d[A][B]

        coords = []
        for i in range(n):
            x = (d[A][i] - d[B][i] + dab) / 2
            coords.append((x, i + 1))

        coords.sort()

        ans = [0] * n
        for i in range(n):
            ans[i] = coords[i][1]

        print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng ma trận khoảng cách cho mỗi trường hợp thử nghiệm. Sau đó, nó xác định điểm cuối A bằng cách chọn nút xa nhất từ điểm bắt đầu tùy ý và điểm cuối B bằng cách tối đa hóa khoảng cách từ A. 

Công thức tọa độ được áp dụng trực tiếp mà không cần thủ thuật làm tròn số nguyên ngoài phép chia cho 2. Vì đầu vào được đảm bảo là số liệu dòng hợp lệ nên tất cả các giá trị trung gian đều nhất quán và phép chia tạo ra số nguyên. 

Cuối cùng, việc sắp xếp theo các tọa độ được xây dựng lại này sẽ mang lại thứ tự chính xác, được ánh xạ trực tiếp lên các vị trí đã sắp xếp. 

Một lỗi triển khai phổ biến là bỏ qua lần quét thứ hai cho điểm cuối B. Nếu không có hai điểm cuối, tọa độ sẽ trở nên mơ hồ và việc sắp xếp suy biến thành cụm không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó gấu trúc nằm ở các vị trí ẩn 0, 3, 7. Ma trận khoảng cách mã hóa những khác biệt này. 

Chúng ta chọn A là nút xa nhất so với một nút tùy ý, giả sử A tương ứng với vị trí 0. Khi đó B trở thành nút ở vị trí 7. 

| Bước | A | B | d[A][i] | d[B][i] | tọa độ x[i] | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | - | - | - | - | 
| chọn A | 0 | - | tối đa từ 0 | - | - | 
| chọn B | 0 | 2 | - | tối đa từ A | - | 
| tính i=0 | 0 | 2 | 0 | 7 | 0 | 
| tính i=1 | 0 | 2 | 3 | 4 | 3 | 
| tính i=2 | 0 | 2 | 7 | 0 | 7 | 

Điều này xác nhận rằng tọa độ tái tạo lại thứ tự ban đầu. 

Bây giờ hãy xem xét trường hợp đối xứng trong đó các điểm cuối bị đảo ngược trong cách diễn giải. Tọa độ được tính toán chỉ đơn giản là lật dấu hoặc dịch chuyển, nhưng việc sắp xếp vẫn giống hệt theo thứ tự ngược lại, điều này vẫn hợp lệ. 

| Bước | A | B | coords (chưa sắp xếp) | sắp xếp thứ tự | 
| --- | --- | --- | --- | --- | 
| lựa chọn điểm cuối | cuối bên phải | đầu bên trái | giá trị đảo ngược | thứ tự đảo ngược | 

Điều này chứng tỏ rằng thuật toán chấp nhận sự phản chiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) mỗi lần kiểm tra | Hai lần quét để tìm điểm cuối cùng với việc xây dựng và sắp xếp tọa độ đầy đủ | 
| Không gian | O(n^2) | Lưu trữ ma trận khoảng cách | 

Tổng n trên tất cả các trường hợp thử nghiệm tối đa là 1000, do đó việc xây dựng bậc hai nằm trong giới hạn. Chi phí chủ yếu là đọc và xử lý ma trận khoảng cách, chứ không phải logic tái thiết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            p = list(map(int, input().split()))
            d = [list(map(int, input().split())) for _ in range(n)]

            if n == 1:
                print(1)
                continue

            A = 0
            for i in range(n):
                if d[0][i] > d[0][A]:
                    A = i

            B = A
            for i in range(n):
                if d[A][i] > d[A][B]:
                    B = i

            dab = d[A][B]

            coords = []
            for i in range(n):
                x = (d[A][i] - d[B][i] + dab) / 2
                coords.append((x, i + 1))

            coords.sort()
            print(*[x[1] for x in coords])

    return ""  # placeholder (not used)

# We only provide structural tests since full runner wiring is omitted in CF style.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | 1 | độ đúng tối thiểu | 
| dòng cách đều nhau | đơn hàng hợp lệ | tái thiết tiêu chuẩn | 
| điểm cuối đảo ngược | hoán vị ngược | phản xạ bất biến | 
| dòng hợp lệ ngẫu nhiên | thứ tự nào đúng | tính đúng đắn chung | 

## Vỏ cạnh 

Khi tất cả các gấu trúc có khoảng cách đều nhau, nhiều gấu trúc có thể chia sẻ các mẫu khoảng cách giống hệt nhau so với trục không có điểm cuối. Việc xây dựng lại dựa trên điểm cuối sẽ tránh được sự mơ hồ này vì các điểm cuối luôn có độ lệch tâm tối đa duy nhất. 

Trong một đường ba điểm nhỏ trong đó điểm giữa được chọn làm tham chiếu ban đầu, việc sắp xếp đơn giản theo khoảng cách không thành công vì cả hai cạnh đều có vẻ đối xứng. Chiến lược điểm cuối giải quyết vấn đề này bằng cách neo từ các chi tiết, buộc sự bất đối xứng vào hệ tọa độ. 

Đối với n = 1, thuật toán phải đoản mạch vì không cần lựa chọn hoặc phân chia điểm cuối và cần xuất trực tiếp gấu trúc đơn để đảm bảo tính chính xác.
