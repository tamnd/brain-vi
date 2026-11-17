---
title: "CF 1969A - Hai Người Bạn"
description: "Mỗi người bạn được chỉ định chính xác một người bạn khác làm “bạn thân” của họ, và nhiệm vụ này tạo thành một hoán vị có kích thước $n$. Nguyên tắc chính là một người bạn chỉ tham dự bữa tiệc nếu cả họ và người bạn thân nhất được chỉ định của họ đều được mời."
date: "2026-06-08T17:42:52+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1969
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 165 (Rated for Div. 2)"
rating: 800
weight: 1969
solve_time_s: 83
verified: true
draft: false
---

[CF 1969A - Hai người bạn](https://codeforces.com/problemset/problem/1969/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người bạn được chỉ định chính xác một người bạn khác làm “bạn thân” của họ, và sự phân công này tạo thành một hoán vị kích thước$n$. Nguyên tắc chính là một người bạn chỉ tham dự bữa tiệc nếu cả họ và người bạn thân nhất được chỉ định của họ đều được mời. 

Điều này tạo ra một biểu đồ phụ thuộc trong đó mỗi đỉnh có chính xác một cạnh đi ra và mỗi nút cũng có chính xác một cạnh đi vào. Do đó, cấu trúc là sự kết hợp rời rạc của các chu kỳ có định hướng. Mỗi chu kỳ đại diện cho một nhóm khép kín phụ thuộc lẫn nhau. 

Chúng tôi được phép gửi lời mời tới bất kỳ nhóm bạn bè nào. Trong số những người được mời, chỉ có một số người thực sự xuất hiện: một người bạn$i$chỉ xuất hiện trong buổi tham dự cuối cùng nếu cả hai$i$Và$p_i$nằm trong danh sách được mời. Mục đích là để đảm bảo có ít nhất hai người bạn tham dự, đồng thời giảm thiểu số lượng lời mời được gửi đi. 

Các ràng buộc là nhỏ, với$n \le 50$, do đó, ngay cả lý luận bậc ba hoặc hàm mũ cho mỗi trường hợp thử nghiệm cũng có thể được chấp nhận. Tuy nhiên, số lượng ca kiểm thử có thể lớn, lên tới 5000, do đó giải pháp phải tuyến tính hoặc gần tuyến tính cho mỗi ca kiểm thử. 

Một trường hợp thất bại ngây thơ nhưng quan trọng xuất phát từ việc cho rằng mời hai người bạn luôn là đủ. Hãy xem xét một 4 chu kỳ như$1 \to 2 \to 3 \to 4 \to 1$. Nếu chúng ta chỉ mời hai nút không phải là bạn thân của nhau, chẳng hạn như 1 và 2, thì sẽ không có ai tham dự vì 1 cần 2 và 2 cần 3. Vì vậy, số người tham dự thực tế trống. Điều này cho thấy “mời hai người” không đồng nghĩa với việc “được hai người tham dự”. 

Một trường hợp tế nhị khác là khi có 2 chu kỳ. Ví dụ$1 \leftrightarrow 2$. Chỉ cần mời 1 và 2 là đủ, cả hai đều tham dự. Điều này gợi ý rằng các chu kỳ nhỏ hoạt động khác với các chu kỳ dài hơn và cấu trúc chu trình là cốt lõi của vấn đề. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử từng tập hợp con của những người bạn được mời, mô phỏng những người tham dự và lấy tập hợp con nhỏ nhất mang lại ít nhất hai người tham dự. Đối với mỗi tập hợp con, chúng tôi kiểm tra từng người bạn để xem liệu họ và người bạn thân nhất của họ có mặt hay không. Đây là$O(2^n \cdot n)$, điều này đã không thể thực hiện được ngay cả đối với$n = 50$, từ$2^{50}$có kích thước lớn về mặt thiên văn. 

Quan sát quan trọng là việc tham dự hoàn toàn được xác định bởi các chu kỳ trong hoán vị. Vì mỗi nút trỏ đến chính xác một nút khác và tất cả các nút đều khác nhau nên biểu đồ sẽ phân tách thành các chu trình. Trong một chu kỳ, việc tham dự bị hạn chế bởi sự liền kề dọc theo chu kỳ đó. 

Đối với chu kỳ có độ dài 2, việc mời cả hai nút ngay lập tức mang lại 2 người tham dự. Đối với bất kỳ chu kỳ nào có độ dài lớn hơn 2, một nút được mời sẽ không truyền bá việc tham dự vì hàng xóm của nó cũng phải được mời và hàng xóm đó phụ thuộc vào một nút khác, tạo ra một chuỗi không thể đóng trừ khi toàn bộ chu trình được xem xét. 

Sự đơn giản hóa quan trọng là câu trả lời chỉ phụ thuộc vào kích thước chu kỳ nhỏ nhất, nhưng có điểm khác biệt đặc biệt: chu kỳ 2 cho câu trả lời 2, trong khi bất kỳ chu kỳ nào có độ dài ít nhất là 3 cho phép chúng tôi có được 2 người tham dự bằng cách mời bất kỳ 3 nút liên tiếp nào trong chu kỳ. Điều đó đảm bảo một chuỗi trong đó hai phụ thuộc liền kề được thỏa mãn đồng thời. 

Do đó, vấn đề giảm xuống còn việc tìm tất cả độ dài chu kỳ và dùng cách tối thiểu có thể để tạo ra ít nhất hai người tham dự, trở thành 2 (nếu có 2 chu kỳ tồn tại) hoặc 3 nếu ngược lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Phân hủy chu trình |$O(n)$mỗi trường hợp thử nghiệm |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và trích xuất các chu trình hoán vị. 

1. Xây dựng một mảng có kích thước được truy cập$n$, ban đầu tất cả đều sai. Điều này đảm bảo chúng tôi không truy cập lại các nút đã được gán cho một chu kỳ. 
2. Lặp lại mọi nút$i$. Nếu nó không được truy cập, hãy bắt đầu theo chuỗi$i \to p_i \to p_{p_i} \to \dots$cho đến khi chúng ta quay trở lại nút đã truy cập. Tất cả các nút gặp phải tạo thành một chu kỳ. Đánh dấu họ đã ghé thăm. 
3. Ghi lại độ dài của mỗi chu kỳ. Điều này là đủ vì cấu trúc hoán vị đảm bảo không phân nhánh, do đó mỗi nút thuộc về chính xác một chu trình. 
4. Nếu bất kỳ chu kỳ nào có độ dài chính xác là 2, thì ngay lập tức trả về 2. Điều này là do chu kỳ 2 cho phép thỏa mãn sự phụ thuộc lẫn nhau với chính xác hai lời mời, một lời mời cho mỗi nút. 
5. Nếu không tồn tại 2 chu kỳ, hãy trả về 3. Bất kỳ chu kỳ nào dài hơn luôn cho phép xây dựng cấu hình hợp lệ với 3 lời mời tạo ra ít nhất hai người tham dự. 

Tính đúng đắn phụ thuộc vào thực tế là mối quan hệ bạn thân chỉ đối xứng theo các chu kỳ có độ dài bằng 2. Các chu kỳ dài hơn yêu cầu phá vỡ các chuỗi phụ thuộc và cấu hình ổn định nhỏ nhất tạo ra sự tham gia cần có ba nút liên tiếp. 

### Tại sao nó hoạt động 

Hoán vị phân rã thành các chu trình độc lập và các ràng buộc tham dự là cục bộ đối với các cạnh. Chu kỳ 2 là cấu trúc duy nhất trong đó hai nút có thể thỏa mãn lẫn nhau mà không liên quan đến bất kỳ nút thứ ba nào. Mỗi chu kỳ có độ dài ít nhất 3 buộc phải có ít nhất một phụ thuộc trung gian, do đó, hai lời mời không đủ để đảm bảo có hai người tham dự trừ khi bản thân chu kỳ đó có kích thước chính xác là 2. Sau khi loại trừ tất cả 2 chu kỳ, cấu trúc tối thiểu kích hoạt hai quan hệ phụ thuộc liền kề cần có ba nút, điều này luôn có thể đạt được trong bất kỳ chu kỳ dài hơn nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = [0] + list(map(int, input().split()))
        
        vis = [False] * (n + 1)
        has_two_cycle = False

        for i in range(1, n + 1):
            if not vis[i]:
                cur = i
                cycle = []
                while not vis[cur]:
                    vis[cur] = True
                    cycle.append(cur)
                    cur = p[cur]

                if len(cycle) == 2:
                    has_two_cycle = True

        if has_two_cycle:
            print(2)
        else:
            print(3)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện phân rã chu trình của hoán vị. Mảng được lập chỉ mục 1 để khớp với định nghĩa bài toán, giúp tránh nhầm lẫn khi theo dõi$p_i$. Mỗi chu kỳ được thu thập bằng cách đi bộ cho đến khi gặp nút đã truy cập trước đó và độ dài của nó chỉ được sử dụng để phát hiện xem có tồn tại 2 chu kỳ hay không. Câu trả lời chỉ phụ thuộc vào việc có xuất hiện chu trình có độ dài bằng hai hay không. 

Một điểm tinh tế là chúng ta không bao giờ cần phải mô phỏng lời mời hoặc sự tham dự một cách rõ ràng. Cấu trúc chu trình xác định đầy đủ tính khả thi, do đó chỉ cần theo dõi độ dài chu kỳ là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 1 2 5 4
```| Nút bắt đầu | Chu kỳ được phát hiện | Độ dài chu kỳ | Tìm thấy 2 chu kỳ | 
| --- | --- | --- | --- | 
| 1 | 1 → 3 → 2 → 1 | 3 | Không | 
| 4 | 4 → 5 → 4 | 2 | Có | 

Điều này cho thấy rằng mặc dù tồn tại 3 chu kỳ nhưng sự hiện diện của 2 chu kỳ chiếm ưu thế trong câu trả lời. Khi đã tồn tại một cặp chung, việc chỉ mời hai người đó sẽ mang lại hai người tham dự, điều này là tối ưu. 

### Ví dụ 2 

đầu vào:```
4
2 3 4 1
```| Nút bắt đầu | Chu kỳ được phát hiện | Độ dài chu kỳ | Tìm thấy 2 chu kỳ | 
| --- | --- | --- | --- | 
| 1 | 1 → 2 → 3 → 4 → 1 | 4 | Không | 

Vì không tồn tại 2 chu kỳ nên cách xây dựng tốt nhất có thể cần có 3 lời mời. Bất kỳ nỗ lực nào với 2 lời mời đều không thành công vì mọi nút đều phụ thuộc vào một nút khác ngoài tập hợp đã chọn. 

Điều này xác nhận rằng chu kỳ dài hơn không thể đạt được hai người tham dự chỉ với hai lời mời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi nút được truy cập chính xác một lần trong khi hình thành chu kỳ | 
| Không gian |$O(n)$| Đã truy cập mảng và lưu trữ chu kỳ tạm thời | 

Các ràng buộc cho phép lên tới 5000 ca kiểm thử, nhưng mỗi ca kiểm thử đều tuyến tính theo$n \le 50$, do đó tổng công việc vẫn còn nhỏ. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n = int(input())
        p = [0] + list(map(int, input().split()))
        
        vis = [False] * (n + 1)
        has_two_cycle = False

        for i in range(1, n + 1):
            if not vis[i]:
                cur = i
                cycle = []
                while not vis[cur]:
                    vis[cur] = True
                    cycle.append(cur)
                    cur = p[cur]
                if len(cycle) == 2:
                    has_two_cycle = True

        print(2 if has_two_cycle else 3)

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("3\n5\n3 1 2 5 4\n4\n2 3 4 1\n2\n2 1") == "2\n3\n2"

# all nodes in 2-cycles
assert run("1\n2\n2 1") == "2"

# single large cycle
assert run("1\n5\n2 3 4 5 1") == "3"

# mixture with 2-cycle
assert run("1\n4\n2 1 4 3") == "2"

# minimum n
assert run("1\n2\n2 1") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 2 | chu kỳ nhỏ nhất có thể | 
| 5 chu kỳ | 3 | vắng lực 2 chu kỳ 3 | 
| chu trình hỗn hợp | 2 | sự thống trị của điều kiện 2 chu kỳ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi hoán vị bao gồm toàn bộ các chu kỳ dài hơn 2. Ví dụ:$1 \to 2 \to 3 \to 4 \to 1$. Thuật toán xác định chính xác một chu kỳ có độ dài 4 và không đặt cờ 2 chu kỳ, tạo ra đầu ra 3. Mọi nỗ lực giảm xuống còn 2 lời mời đều thất bại vì không cặp nào có thể đáp ứng sự phụ thuộc lẫn nhau. 

Một trường hợp khác là khi tồn tại nhiều chu kỳ và chỉ một trong số chúng là 2 chu kỳ, chẳng hạn như$1 \leftrightarrow 2$Và$3 \to 4 \to 5 \to 3$. Thuật toán phát hiện 2 chu kỳ trong thành phần đầu tiên và kết luận ngay câu trả lời là 2. Điều này đúng vì các chu kỳ là độc lập và một cặp hợp lệ là đủ để đáp ứng yêu cầu của ít nhất hai người tham dự. 

Một trường hợp tế nhị cuối cùng là khi$n = 2$. Hoán vị duy nhất có thể xảy ra là 2 chu kỳ, vì vậy câu trả lời luôn là 2. Thuật toán xử lý điều này một cách tự nhiên mà không cần viết hoa đặc biệt vì logic phát hiện chu trình đánh dấu trực tiếp chu kỳ có độ dài hai.
