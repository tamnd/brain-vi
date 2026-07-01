---
title: "CF 104414E - \u6765\u81ea 2020 \u7684\u9c9c\u82b1"
description: "Chúng ta đang làm việc với một biểu đồ hình ngôi sao với các nút $n$, trong đó một nút đóng vai trò là tâm và các nút $n-1$ còn lại là các nút lá. Mỗi chiếc lá được liên kết với một “màu sơn gốc” cố định và có chính xác $n-1$ màu riêng biệt, mỗi màu một chiếc."
date: "2026-06-30T20:02:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104414
codeforces_index: "E"
codeforces_contest_name: "2023 Hunan Provincal Multi-University Training (Xiangtan University)"
rating: 0
weight: 104414
solve_time_s: 61
verified: true
draft: false
---

[CF 104414E - \u6765\u81ea 2020 \u7684\u9c9c\u82b1](https://codeforces.com/problemset/problem/104414/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một biểu đồ hình ngôi sao với$n$các nút, trong đó một nút đóng vai trò là trung tâm và nút còn lại$n-1$nút là lá. Mỗi chiếc lá được liên kết với một “màu sơn gốc” cố định và có chính xác$n-1$màu sắc riêng biệt, mỗi lá một màu. 

Ban đầu, mọi nút đều không có màu. Chúng tôi được phép thực hiện hai loại hoạt động. Đầu tiên, chúng ta có thể kích hoạt một chiếc lá và gán cho nó màu sắc được xác định trước. Thứ hai, khi một nút đã được tô màu, chúng ta có thể sao chép màu của nó sang nút liền kề. Điều này có nghĩa là màu sắc có thể chảy dọc theo các cạnh, nhưng chỉ ra bên ngoài từ các đỉnh đã được tô màu. 

Việc tô màu được coi là hợp lệ nếu mọi nút đều được tô màu sau một số chuỗi thao tác này. Hai màu cuối cùng được coi là khác nhau nếu ít nhất một nút có màu khác. 

Nhiệm vụ là đếm xem có thể tạo ra bao nhiêu màu đầy đủ cuối cùng riêng biệt theo các quy tắc này, modulo$10^9 + 7$, với nhiều giá trị của$n$, Ở đâu$n$có thể lớn như$10^7$và số lượng ca kiểm thử có thể lớn bằng$10^5$. 

Ràng buộc ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào phụ thuộc vào việc lặp qua các nút hoặc các hoạt động mô phỏng đều là không thể. Thậm chí$O(n)$mỗi trường hợp thử nghiệm sẽ quá chậm. Câu trả lời phải giảm xuống một biểu thức dạng đóng có thể được đánh giá theo thời gian không đổi cho mỗi truy vấn, có thể liên quan đến lũy thừa mô-đun hoặc công thức tổ hợp trực tiếp. 

Một điểm tinh tế trong quá trình này là màu sắc không được tự do gán ngay từ đầu; chúng chỉ bắt nguồn từ lá. Tuy nhiên, khi một chiếc lá được kích hoạt, màu của nó có thể lan ra khắp trung tâm và có khả năng ghi đè lên những chiếc lá khác. Sự tương tác này làm cho việc suy luận về các ràng buộc trở nên không cần thiết, vì màu sắc có thể lan truyền và sau đó bị ghi đè lại. 

Trường hợp một cạnh bộc lộ sự nhầm lẫn là$n=3$. Có hai lá có màu sắc cố định. Ngay cả trong trường hợp không tầm thường nhỏ nhất này, trung tâm có thể tạm thời lấy màu của một chiếc lá và sau đó chuyển sang màu khác, tạo ra các cấu hình cuối cùng khác nhau tùy thuộc vào thời gian. Điều này cho thấy cấu trúc cuối cùng không phải là sự lan truyền một nguồn đơn giản mà cho phép ghi đè nhiều lần độc lập. 

## Phương pháp tiếp cận 

Việc giải thích bằng vũ lực sẽ mô phỏng tất cả các chuỗi hoạt động có thể xảy ra. Ở mỗi bước, chúng ta kích hoạt một chiếc lá hoặc truyền màu dọc theo một cạnh. Điều này xây dựng một không gian trạng thái khổng lồ trong đó mỗi nút có thể ở nhiều trạng thái trung gian và quá trình chuyển đổi phụ thuộc vào các lựa chọn trước đó. Ngay cả đối với nhỏ$n$, số lượng chuỗi tăng theo cấp số nhân, vì chúng tôi đang khám phá một cách hiệu quả tất cả các cách để sắp xếp sự truyền màu và đổi màu màu. 

Quan sát cấu trúc quan trọng là đồ thị là một ngôi sao, do đó mọi tương tác giữa các lá đều phải đi qua tâm. Điều này có nghĩa là trung tâm là điểm pha trộn duy nhất của màu sắc. Khi một màu lá đạt đến trung tâm, nó có thể được phân phối lại cho bất kỳ lá nào khác. Ngược lại, bất kỳ chiếc lá nào sau đó cũng có thể có lại màu sắc ban đầu của chính nó. 

Điều này tạo ra hiệu ứng tách rời: màu cuối cùng của mỗi nút không bị ràng buộc bởi cây truyền cố định, vì màu sắc có thể được đưa lại tùy ý nhiều lần thông qua tâm. Kết quả là, mỗi nút hoạt động gần như độc lập đối với lựa chọn màu cuối cùng của nó, miễn là màu đó là một trong những$n-1$màu sắc của lá. 

Điều này làm giảm cấu trúc tổ hợp toàn cục thành một bài toán đếm đơn giản: mỗi$n$các nút có thể độc lập kết thúc ở bất kỳ$n-1$màu sắc có sẵn. Phần trung tâm không có hạn chế đặc biệt nào ngoài việc được tô màu theo màu của lá và các lá không bị hạn chế giữ lại màu gốc của chúng. 

Do đó, tổng số màu hợp lệ sẽ trở thành phép tính thuần túy của các phép gán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Lập luận độc lập kết hợp |$O(1)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng tôi chuyển cấu trúc thành một tính toán trực tiếp. 

1. Quan sát rằng có chính xác$n-1$màu sắc riêng biệt có sẵn, mỗi màu có nguồn gốc từ một lá. 
2. Nhận thức được rằng thông qua việc sử dụng lặp đi lặp lại trung tâm làm trung tâm chuyển giao, bất kỳ nút nào cuối cùng cũng có thể có được bất kỳ thứ nào trong số này$n-1$màu sắc. 
3. Kết luận rằng trạng thái cuối cùng không phụ thuộc vào chuỗi thao tác, chỉ phụ thuộc vào việc gán màu cuối cùng cho các nút. 
4. Đối với mỗi$n$các nút, độc lập chọn một trong các nút$n-1$màu sắc có sẵn. 
5. Nhân các lựa chọn trên tất cả các nút, thu được$(n-1)^n$. 
6. Trả về giá trị này theo modulo$10^9 + 7$. 

Bước lập luận thiết yếu là trung tâm không hạn chế tính nhất quán toàn cầu. Bất kỳ cấu trúc tạm thời nào được tạo trong quá trình nhân giống đều có thể bị ghi đè sau đó, nghĩa là không có biểu đồ phụ thuộc liên tục giữa các lá. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi màu đều bắt nguồn từ một chiếc lá và luôn có thể được đưa lại vào hệ thống bất kỳ lúc nào. Bởi vì tâm có thể được đổi màu tùy ý nhiều lần và sau đó được sử dụng để đổi màu bất kỳ lá nào, nên không có nút nào bị hạn chế vĩnh viễn bởi các lựa chọn truyền trước đó. Điều này loại bỏ mọi sự phụ thuộc giữa các màu nút cuối cùng, do đó không gian cấu hình cuối cùng chính xác là tập hợp tất cả các phép gán từ các nút đến nút$n-1$màu sắc có sẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

t = int(input())
for _ in range(t):
    n = int(input())
    print(modpow(n - 1, n))
```Việc triển khai làm giảm toàn bộ vấn đề thành lũy thừa mô-đun nhanh. Mỗi trường hợp thử nghiệm tính toán$(n-1)^n \bmod (10^9+7)$. Hàm lũy thừa nhị phân đảm bảo rằng ngay cả đối với$n$lên đến$10^7$, tính toán vẫn hiệu quả. 

Một cạm bẫy phổ biến ở đây là trộn cơ số và số mũ, vì cả hai đều phụ thuộc vào$n$. Một vấn đề tinh tế khác là đảm bảo rằng phép lũy thừa chỉ được thực hiện theo mô đun cho các phép nhân trung gian chứ không phải cho chính số mũ. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 3$. Chúng tôi có 2 màu và 3 nút. Công thức cho$2^3 = 8$. 

| Nút | Lựa chọn màu sắc | 
| --- | --- | 
| 1 (giữa) | 1 hoặc 2 | 
| 2 | 1 hoặc 2 | 
| 3 | 1 hoặc 2 | 

Điều này xác nhận rằng tất cả các kết hợp đều được tính, mang lại 8 cấu hình. 

Bây giờ hãy xem xét$n = 4$, nơi có 3 màu và 4 nút. Công thức cho$3^4 = 81$. Mỗi nút chọn độc lập một trong ba màu lá có sẵn và tất cả các phép gán như vậy đều có thể truy cập được thông qua quá trình lan truyền qua trung gian lặp đi lặp lại. 

Những ví dụ này xác nhận rằng mô hình hoạt động giống như một bài toán gán không giới hạn trên một tập hợp màu cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log n)$| Mỗi truy vấn sử dụng lũy ​​thừa nhị phân trên số mũ$n$| 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì mỗi trường hợp thử nghiệm được xử lý độc lập theo thời gian logarit và không yêu cầu mô phỏng trên mỗi nút ngay cả đối với các trường hợp rất lớn.$n$. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def modpow(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(modpow(n - 1, n)))
    return "\n".join(out)

# minimum case
assert run("1\n3\n") == "8"

# slightly larger
assert run("1\n4\n") == "81"

# multiple tests
assert run("3\n3\n4\n5\n") == "\n".join([
    str(pow(2,3,MOD)),
    str(pow(3,4,MOD)),
    str(pow(4,5,MOD))
])

# larger structure check
assert run("1\n2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=2$| 1 | cấu trúc hợp lệ nhỏ nhất | 
|$n=3$| 8 | ngôi sao không tầm thường đầu tiên | 
| nhiều$n$| quyền lực hỗn hợp | tính nhất quán giữa các truy vấn | 

## Vỏ cạnh 

cho$n=2$, chỉ có một lá và một tâm. Công thức cho$1^2 = 1$, nghĩa là chỉ có một cách tô màu duy nhất. Điều này phù hợp với thực tế là cả hai nút phải có màu duy nhất có sẵn. 

Đối với lớn hơn$n$, chẳng hạn như$n=10^7$, tính toán không thay đổi về cấu trúc. Phép lũy thừa chạy theo thời gian logarit và không có trạng thái trung gian nào phụ thuộc vào kích thước đồ thị ngoài các tham số số học, do đó thuật toán vẫn ổn định ngay cả ở giới hạn đầu vào cực đoan. 

Giả định về tính độc lập cũng đúng trong những trường hợp cực đoan này vì cấu trúc sao đảm bảo rằng tất cả các tương tác vẫn đi qua tâm mà không đưa ra các ràng buộc bổ sung giữa các lá.
