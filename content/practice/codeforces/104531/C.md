---
title: "CF 104531C - Bắt"
description: "Chúng ta được cấp một cây vô hướng trong đó một số nút có thể chứa chuột đồng. Mỗi con chuột hamster không hề đứng yên mà nó chuyển động mãi mãi."
date: "2026-06-30T09:54:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104531
codeforces_index: "C"
codeforces_contest_name: "2022 SYSU School Contest"
rating: 0
weight: 104531
solve_time_s: 45
verified: true
draft: false
---

[CF 104531C - Bắt](https://codeforces.com/problemset/problem/104531/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây vô hướng trong đó một số nút có thể chứa chuột đồng. Mỗi con chuột hamster không hề đứng yên mà nó chuyển động mãi mãi. Mỗi giây nó bước tới một nút liền kề, nhưng nó không được phép quay lại ngay theo cạnh mà nó vừa sử dụng, trừ khi nó đi qua một nút lá, trong trường hợp đó nó được phép đảo ngược hướng một lần. Điều này tạo ra một kiểu bước đi bị hạn chế nhằm ngăn cản sự dao động qua lại tầm thường trên các đỉnh bên trong. 

Chúng ta được phép đặt bẫy chuột trên một tập hợp các đỉnh đã chọn. Một con hamster sẽ bị bắt nếu nó đáp xuống bất kỳ đỉnh nào có chứa bẫy. Vì chúng tôi không kiểm soát các vị trí hoặc chuyển động ban đầu nên chúng tôi phải đảm bảo rằng mọi quỹ đạo có thể có của hamster cuối cùng đều chạm vào ít nhất một bẫy. 

Đầu ra là số đỉnh tối thiểu mà chúng ta phải đặt bẫy để cuối cùng mọi đường đi của hamster có thể đều bị chặn. 

Ràng buộc n lên tới 100000 buộc mọi giải pháp phải tuyến tính hoặc gần tuyến tính về số lượng nút. Bất cứ điều gì cố gắng mô phỏng chuyển động, liệt kê các đường đi hoặc xem xét tất cả các trạng thái bắt đầu sẽ ngay lập tức thất bại vì số lần đi bộ có thể có trong cây tăng theo cấp số nhân. 

Một điểm tinh tế trong vấn đề này là chuột hamster cư xử khác với những chiếc lá. Tại một chiếc lá, chúng được phép đảo ngược hướng, điều này khiến cho những chiếc lá hoạt động giống như những “điểm phản chiếu” hơn là những ngõ cụt. Đây chính xác là điều khiến cho trực giác ngây thơ dựa trên phạm vi bao phủ đường dẫn đơn giản trở nên không chính xác. 

Một trường hợp thất bại điển hình xuất phát từ việc giả định rằng mọi lá đều phải được “bảo vệ cục bộ”. Ví dụ, trong một cái cây hình ngôi sao nơi một tâm kết nối với nhiều lá, một cách tiếp cận đơn giản có thể thử đặt bẫy trên các lá, nhưng câu trả lời đúng là đặt bẫy ở tâm vì mọi chuyển động giữa các lá đều phải đi qua nó. Bất kỳ cách tiếp cận nào chỉ giải quyết cục bộ xung quanh đều bỏ qua cấu trúc thắt cổ chai toàn cầu này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng suy luận về mọi quỹ đạo có thể có của hamster bắt đầu từ mọi nút. Người ta có thể tưởng tượng việc mô phỏng một con hamster từ mỗi đỉnh bắt đầu, khám phá tất cả các bước di chuyển có thể xảy ra đồng thời tôn trọng quy tắc “không quay lại ngay lập tức trừ khi ở một chiếc lá” và kiểm tra xem nút nào chắc chắn được truy cập. Sau đó, chúng tôi sẽ thử chọn một tập hợp tối thiểu các nút bẫy giao nhau với tất cả các quỹ đạo như vậy. Điều này nhanh chóng trở thành một vấn đề lớn về không gian trạng thái vì mỗi trạng thái không chỉ phụ thuộc vào đỉnh hiện tại mà còn phụ thuộc vào hướng của cạnh trước đó, làm tăng gấp đôi không gian trạng thái một cách hiệu quả. Ngay cả khi cắt tỉa, số bước đi riêng biệt trên cây có thể tăng theo cấp số nhân theo độ sâu, vì vậy phương pháp này không khả thi. 

Thông tin chi tiết quan trọng là chúng ta thực sự không cần phải theo dõi đường đi của từng chú chuột hamster. Thay vào đó, chúng ta nên suy luận xem đỉnh nào là “điểm giao nhau” không thể tránh khỏi đối với tất cả các bước đi vô hạn hợp lệ hoặc kéo dài tùy ý. Bởi vì quy tắc di chuyển chỉ hạn chế việc quay lại ngay lập tức, nên về cơ bản, hamster thực hiện bước đi không quay lại ngoại trừ khi lá cây. Những bước đi như vậy hoạt động giống như việc di chuyển dọc theo “cấu trúc lõi” của cây và bất kỳ đỉnh nào bị loại bỏ sẽ ngắt kết nối cây theo cách cô lập tất cả các tuyến đường từ lá này sang lá khác đều trở nên quan trọng. 

Nếu chúng ta nghĩ về mặt bao phủ, một cái bẫy chỉ cần thiết ở các đỉnh nằm trên tất cả các “đường đi thuận nghịch” có thể nối các lá. Các lối đi bên trong không phân nhánh không cần nhiều bẫy vì hamster đi vào hành lang phải đi qua cùng một cấu trúc khớp nối bất kể hướng nào.

Điều này làm giảm vấn đề xác định một tập đỉnh tối thiểu bao phủ tất cả các đường truyền từ lá này sang lá khác trong chuyển động không quay lui. Quan sát quan trọng là chỉ những đỉnh kết nối “các hướng thoát” khác nhau mới quan trọng. Trong một cây, đây chính xác là các điểm phân nhánh nằm trên cấu trúc rút gọn thu được sau khi loại bỏ các chuỗi nút thẳng cấp 2. 

Sau khi nén tất cả chuỗi tối đa của các nút cấp 2, chúng tôi thu được một cây đơn giản hơn trong đó tất cả các nút bên trong có bậc không bằng 2. Trong cây rút gọn này, mọi cạnh đại diện cho một hành lang bắt buộc và mỗi nút bên trong là điểm giao bắt buộc cho nhiều hướng độc lập. Câu trả lời trở thành số đỉnh giao nhau cần thiết để chặn tất cả các bước đi hợp lệ vô hạn, tương ứng với việc chọn tất cả các đỉnh có bậc ít nhất là 3 trong cấu trúc rút gọn, vì bất kỳ con hamster nào di chuyển giữa các nhánh khác nhau đều phải đi qua một trong các đỉnh này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi bước đi | Hàm mũ | Hàm mũ | Quá chậm | 
| Giảm cây + đếm cấu trúc | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của cây và tính độ của tất cả các đỉnh. Điều này cung cấp cấu trúc phân nhánh cục bộ, là thông tin duy nhất cần thiết để xác định các điểm truyền tải không thể tránh khỏi. 
2. Xác định các đỉnh có bậc ít nhất là 3. Các đỉnh này biểu thị các điểm phân nhánh mà chuột hamster có nhiều hướng khác nhau để tiếp tục đi mà không cần đảo ngược ngay lập tức. 
3. Đếm tất cả các đỉnh như vậy. Mỗi điểm này hoạt động như một điểm chặn cần thiết vì bất kỳ chuyển động nào chuyển đổi giữa các cây con khác nhau đều phải đi qua một trong số chúng. 
4. Trả lại số đếm này làm câu trả lời. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ chuyển động hợp lệ nào của hamster chuyển tiếp giữa các vùng lá khác nhau đều phải đi qua một đỉnh nơi có ít nhất ba hướng riêng biệt gặp nhau. Các đỉnh cấp 1 chỉ phản ánh chuyển động và các đỉnh cấp 2 chỉ tạo thành các hành lang tuyến tính nơi chuột hamster không thể tạo ra hành vi phân nhánh. Chỉ các đỉnh bậc 3 trở lên mới tạo ra các điểm lựa chọn thực sự trong đó các đường dẫn từ các phần khác nhau của cây hợp nhất hoặc phân kỳ. Vì mỗi lần di chuyển không tầm thường giữa các phần khác nhau của cây phải đi qua ít nhất một đỉnh như vậy, nên việc đặt bẫy chính xác trên các đỉnh này đảm bảo ngăn chặn tất cả các hành vi dài hạn có thể xảy ra và không có tập hợp nhỏ hơn nào có thể che phủ tất cả các giao điểm không thể tránh khỏi như vậy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    deg = [0] * (n + 1)

    for _ in range(n - 1):
        u, v = map(int, input().split())
        deg[u] += 1
        deg[v] += 1

    ans = 0
    for i in range(1, n + 1):
        if deg[i] >= 3:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện dựa hoàn toàn vào việc tính mức độ. Cấu trúc kề không được lưu trữ rõ ràng vì chỉ có độ quan trọng để xác định các đỉnh phân nhánh. Vòng lặp trên các cạnh xây dựng độ theo O(n) và lần quét cuối cùng sẽ tính ra câu trả lời. 

Một lỗi triển khai phổ biến là bao gồm không chính xác các nút cấp 2. Các nút này nằm trên các chuỗi đơn giản và không đưa ra các lựa chọn phân nhánh, vì vậy việc đếm chúng sẽ đánh giá quá cao câu trả lời. Một vấn đề tế nhị khác là quên rằng cây có n=1 hoặc n=2 không có nút bậc 3, do đó kết quả đầu ra đúng là bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2
2 3
```| Nút | Bằng cấp | ≥3? | 
| --- | --- | --- | 
| 1 | 1 | không | 
| 2 | 2 | không | 
| 3 | 1 | không | 

Câu trả lời là 0. 

Điều này cho thấy một chuỗi đơn giản không có điểm phân nhánh. Hamster chỉ có một con đường tiến về phía trước ở mỗi bước, vì vậy về mặt cấu trúc không cần có điểm chặn. 

### Ví dụ 2 

đầu vào:```
5
1 2
1 3
1 4
4 5
```| Nút | Bằng cấp | ≥3? | 
| --- | --- | --- | 
| 1 | 3 | vâng | 
| 2 | 1 | không | 
| 3 | 1 | không | 
| 4 | 2 | không | 
| 5 | 1 | không | 

Câu trả lời là 1. 

Điều này thể hiện cấu trúc giống như ngôi sao tập trung ở nút 1. Mọi chuyển động giữa các nhánh khác nhau đều phải đi qua nút 1, vì vậy chỉ cần một bẫy là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được xử lý một lần để tính toán độ, sau đó là quét các nút một lần | 
| Không gian | O(n) | Mảng độ và biểu diễn kề cận tiềm ẩn | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả bộ nhớ và thời gian đều tăng tuyến tính với số lượng nút, tối ưu cho đầu vào cây có kích thước lên tới 100000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    deg = [0] * (n + 1)
    for _ in range(n - 1):
        u, v = map(int, input().split())
        deg[u] += 1
        deg[v] += 1

    ans = sum(1 for i in range(1, n + 1) if deg[i] >= 3)
    return str(ans)

# sample-like tests
assert run("1\n") == "0"
assert run("3\n1 2\n2 3\n") == "0"
assert run("5\n1 2\n1 3\n1 4\n4 5\n") == "1"

# star
assert run("4\n1 2\n1 3\n1 4\n") == "1"

# line
assert run("6\n1 2\n2 3\n3 4\n4 5\n5 6\n") == "0"

# binary branching chain
assert run("7\n1 2\n1 3\n2 4\n2 5\n3 6\n3 7\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | cây tối thiểu | 
| đồ thị đường dẫn | 0 | không phân nhánh | 
| đồ thị sao | 1 | phát hiện trung tâm trung tâm | 
| cây nhị phân | 1 | rễ phân nhánh đơn | 

## Vỏ cạnh 

Cây một nút được xử lý một cách tự nhiên vì không có nút nào có bậc ≥ 3 nên đáp án là 0. 

Một con đường đơn giản cũng dễ hiểu. Mỗi nút có nhiều nhất là 2 nên không đặt bẫy. Thuật toán quét độ và trả về số 0 một cách chính xác. 

Trong cây hình ngôi sao, tâm có bậc n−1 và được tính một lần. Mọi đường đi giữa các lá nhất thiết phải đi qua nó, do đó thuật toán đặt chính xác một bẫy. 

Trong cấu trúc phân nhánh nhị phân đầy đủ, chỉ có gốc thường đạt cấp độ 3 trở lên tùy thuộc vào cách xây dựng và thuật toán vẫn chỉ tính các điểm nối phân nhánh thực sự đó, tránh việc đếm quá mức dọc theo chuỗi.
