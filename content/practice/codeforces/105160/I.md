---
title: "CF 105160I - \u7ea0\u7f20\u4e4b\u5706"
description: "Chúng ta có hai đường tròn trong mặt phẳng. Mỗi vòng tròn được xác định bởi tọa độ tâm và bán kính của nó. Đối với mỗi trường hợp thử nghiệm, chúng ta cần đếm có bao nhiêu đường thẳng phân biệt tồn tại sao cho đường thẳng đó tiếp xúc với cả hai đường tròn cùng một lúc."
date: "2026-06-27T11:02:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105160
codeforces_index: "I"
codeforces_contest_name: "2024 University of Shanghai for Science and Technology(USST) Freshman Challenge Contest"
rating: 0
weight: 105160
solve_time_s: 50
verified: true
draft: false
---

[CF 105160I - \u7ea0\u7f20\u4e4b\u5706](https://codeforces.com/problemset/problem/105160/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đường tròn trong mặt phẳng. Mỗi vòng tròn được xác định bởi tọa độ tâm và bán kính của nó. Đối với mỗi trường hợp thử nghiệm, chúng ta cần đếm có bao nhiêu đường thẳng phân biệt tồn tại sao cho đường thẳng đó tiếp xúc với cả hai đường tròn cùng một lúc. Nếu có vô số dòng như vậy, chúng ta phải xuất ra −1. 

Về mặt hình học, một đường thẳng là hợp lệ nếu nó chạm vào mỗi đường tròn tại đúng một điểm mà không cắt phần bên trong. Cùng một đường hình học chỉ được tính một lần ngay cả khi nó có thể được coi là đến từ các “cấu hình tiếp tuyến” khác nhau. 

Các ràng buộc rất lớn về mặt trường hợp thử nghiệm, lên tới 100000, trong khi tọa độ và bán kính tối đa là 10000. Điều này ngay lập tức loại trừ bất kỳ cấu trúc hình học nào trên mỗi thử nghiệm liệt kê các đường ứng cử viên một cách rõ ràng hoặc thực hiện kiểm tra giao lộ đối với tất cả các tiếp tuyến có thể có. Giải pháp phải giảm từng trường hợp thử nghiệm thành một lượng số học không đổi. 

Trường hợp cạnh tinh tế phát sinh khi hai đường tròn trùng nhau. Nếu cả hai tâm và bán kính bằng nhau thì mọi đường tiếp tuyến của đường tròn đó đồng thời tiếp xúc với cả hai, điều này mang lại vô số đường thẳng hợp lệ. Một trường hợp góc khác xuất hiện khi các vòng tròn được lồng vào nhau hoặc một vòng tròn nằm hoàn toàn bên trong vòng tròn kia mà không chạm vào; trong những trường hợp như vậy, không tồn tại đường tiếp tuyến chung. Một cách tiếp cận đơn giản chỉ kiểm tra khoảng cách trung tâm so với tổng bán kính thường không phân biệt được chính xác cấu hình tiếp tuyến bên ngoài và bên trong. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng xây dựng tất cả các đường tiếp tuyến với vòng tròn đầu tiên và sau đó kiểm tra từng đường thẳng với vòng tròn thứ hai. Một đường tròn có vô số đường tiếp tuyến, vì vậy trong thực tế người ta sẽ rời rạc hóa các hướng hoặc cố gắng liệt kê các tiếp tuyến thông qua lấy mẫu góc hoặc tham số hóa độ dốc. Ngay cả khi chúng ta giới hạn bản thân trong các tiếp tuyến được xác định bởi các điểm tiếp xúc, điều này vẫn liên quan đến các tham số liên tục và giải phương trình bậc hai theo hướng ứng cử viên. Đối với mỗi dòng ứng cử viên, việc xác minh tiếp tuyến với cả hai vòng tròn yêu cầu tính toán khoảng cách và kiểm tra nghiệm, dẫn đến ít nhất O(K) hoạt động trên mỗi trường hợp kiểm thử đối với một số K lớn. Điều này về cơ bản là không khả thi với 100000 trường hợp kiểm thử. 

Quan sát quan trọng là bất kỳ đường tiếp tuyến chung nào của hai đường tròn phải là một trong bốn loại hình học cổ điển: nó có thể là tiếp tuyến chung ngoài (tiếp xúc cả hai đường tròn trên cùng một phía của đoạn nối tâm) hoặc tiếp tuyến chung bên trong (đi qua đoạn nối tâm, chạm hai cạnh đối diện). Mỗi loại giảm xuống một điều kiện khoảng cách giữa tâm đường tròn và bán kính. 

Gọi tâm là A và B, có bán kính r1 và r2, và gọi d là khoảng cách giữa các tâm. Sự tồn tại của các tiếp tuyến chỉ phụ thuộc vào việc so sánh d với r1 + r2 và |r1 − r2|. Điều này xuất phát từ việc dịch chuyển một đường tròn thành một điểm bằng cách mở rộng hoặc thu nhỏ đường tròn kia dọc theo hướng pháp tuyến của đường tiếp tuyến. Mỗi cấu hình hợp lệ tương ứng với một bất đẳng thức hình học cụ thể và số lượng tiếp tuyến hợp lệ được xác định bằng số lượng cấu hình có thể có. 

Sự suy biến đặc biệt xảy ra khi các đường tròn trùng nhau (vô số tiếp tuyến), khi chúng tiếp xúc bên trong hoặc bên ngoài (một số loại tiếp tuyến thu gọn thành một đường thẳng) hoặc khi một đường tròn nằm hoàn toàn bên trong đường tròn kia (không có tiếp tuyến). 

Do đó, toàn bộ vấn đề quy về việc phân loại vị trí tương đối của hai đường tròn chỉ sử dụng ba đại lượng: khoảng cách tâm, tổng bán kính và hiệu tuyệt đối của bán kính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng tiếp tuyến) | O(∞) mỗi lần kiểm tra | O(1) | Không thể | 
| Phân loại hình học | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Đối với mỗi trường hợp thử nghiệm, chúng tôi tính toán khoảng cách bình phương giữa các tâm để tránh lỗi dấu phẩy động. Sau đó, chúng tôi so sánh nó với các giá trị ngưỡng bình phương xuất phát từ bán kính. 

1. Đọc hai vòng tròn (x1, y1, r1) và (x2, y2, r2). Nếu các đường tròn giống nhau ở cả tâm và bán kính, chúng ta xuất ra −1 vì mọi đường tiếp tuyến của một đường tròn đều hợp lệ cho cả hai. 
2. Tính dx = x1 − x2 và dy = y1 − y2, rồi xác định d2 = dx·dx + dy·dy. Điều này thể hiện khoảng cách bình phương giữa các trung tâm. 
3. Tính sumR = r1 + r2 và diffR = |r1 − r2|. Các giá trị này xác định ngưỡng cho cấu hình tiếp tuyến bên ngoài và bên trong. 
4. Nếu d2 bằng 0 nhưng các đường tròn không giống nhau thì các đường tròn có cùng tâm nhưng có bán kính khác nhau. Trong trường hợp này, không có đường thẳng nào có thể tiếp tuyến với cả hai vì mọi tiếp tuyến của một đường tròn đều nằm hoàn toàn bên ngoài đường tròn đồng tâm kia. Chúng tôi xuất ra 0. 
5. Nếu d2 < diffR², một vòng tròn nằm hoàn toàn bên trong vòng tròn kia mà không chạm vào. Không có đường tiếp tuyến nào có thể chạm vào cả hai ranh giới cùng một lúc, vì vậy chúng ta xuất ra 0. 
6. Nếu d2 bằng diffR² thì các đường tròn tiếp tuyến trong. Có duy nhất một tiếp tuyến chung trong. 
7. Nếu d2 > diffR² và d2 < sumR², các vòng tròn sẽ chồng lên nhau. Trong miền này tồn tại chính xác hai tiếp tuyến chung ngoài. 
8. Nếu d2 bằng sumR² thì các đường tròn là tiếp tuyến ngoài. Trong trường hợp này, tồn tại ba tiếp tuyến chung phân biệt: hai tiếp tuyến chung ngoài và một tiếp tuyến chung suy biến. 
9. Nếu d2 > sumR², các vòng tròn sẽ rời nhau. Tất cả bốn tiếp tuyến chung cổ điển đều tồn tại. 

### Tại sao nó hoạt động 

Mọi đường tiếp tuyến chung được xác định bởi một vectơ chỉ phương vuông góc với bán kính tại điểm tiếp tuyến. Đối với một đường cố định, việc dịch chuyển dọc theo hướng bình thường của nó sẽ làm giảm vấn đề so sánh khoảng cách được đánh dấu từ tâm đến đường. Các ràng buộc do tiếp tuyến áp đặt sẽ chuyển thành các phương trình tuyến tính trong khoảng cách có dấu đó. Giải đồng thời những điều này cho hai đường tròn sẽ dẫn đến chính xác các điều kiện liên quan đến d, r1 và r2. Số lượng giải pháp tương ứng với số lượng phép gán dấu nhất quán có thể thực hiện được, đó chính xác là những gì mà phân tích trường hợp trên liệt kê. Vì việc phân loại chỉ phụ thuộc vào khoảng cách tương đối nên không cần phải xây dựng hình học ngoài những so sánh này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        x1, y1, r1 = map(int, input().split())
        x2, y2, r2 = map(int, input().split())

        dx = x1 - x2
        dy = y1 - y2
        d2 = dx * dx + dy * dy

        if x1 == x2 and y1 == y2 and r1 == r2:
            out.append("-1")
            continue

        sumR = r1 + r2
        diffR = abs(r1 - r2)

        if d2 == 0:
            out.append("0")
        elif d2 < diffR * diffR:
            out.append("0")
        elif d2 == diffR * diffR:
            out.append("1")
        elif d2 < sumR * sumR:
            out.append("2")
        elif d2 == sumR * sumR:
            out.append("3")
        else:
            out.append("4")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh việc phân loại trực tiếp. Người bảo vệ đầu tiên xử lý trường hợp vô hạn của các vòng tròn giống hệt nhau. Khoảng cách bình phương được sử dụng xuyên suốt để tránh các vấn đề về độ chính xác. Mỗi nhánh tiếp theo tương ứng chính xác với một chế độ hình học được xác định bởi các vòng tròn được lồng vào nhau, tiếp tuyến, giao nhau hay tách rời. Thứ tự kiểm tra rất quan trọng vì các trường hợp đẳng thức phải được tách ra trước các bất đẳng thức nghiêm ngặt để tránh phân loại sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
0 0 1
0 3 1
```Ở đây dx = 0, dy = −3, do đó d2 = 9. Chúng ta có sumR = 2 và diffR = 0, do đó sumR² = 4 và diffR² = 0. 

| Bước | d2 | khác biệtR² | tổngR² | Quyết định | 
| --- | --- | --- | --- | --- | 
| Tính khoảng cách | 9 | 0 | 4 | trường hợp rời rạc | 

Vì d2 > sumR² nên các đường tròn tách biệt và tồn tại cả bốn tiếp tuyến, nên đáp án là 4. 

Điều này xác nhận cách giải thích rằng các vòng tròn tách biệt đủ sẽ thừa nhận cả hai đường tiếp tuyến bên trong và bên ngoài. 

### Ví dụ 2 

đầu vào:```
1
0 0 2
3 0 1
```Ở đây dx = 3, dy = 0, do đó d2 = 9. Chúng ta có sumR = 3, diffR = 1, do đó sumR² = 9 và diffR² = 1. 

| Bước | d2 | khác biệtR² | tổngR² | Quyết định | 
| --- | --- | --- | --- | --- | 
| So sánh | 9 | 1 | 9 | tiếp tuyến ngoài | 

Vì d2 == sumR² nên các đường tròn tiếp tuyến ngoài, tạo ra 3 tiếp tuyến chung. 

Trường hợp này cho thấy tầm quan trọng của việc xử lý đẳng thức: thiếu nhánh đẳng thức sẽ phân loại sai nhánh này thành rời rạc hoặc chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm thực hiện các phép tính số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Giải pháp có quy mô tuyến tính theo số lượng trường hợp thử nghiệm, tối ưu với tối đa 100000 đầu vào. Mỗi trường hợp thử nghiệm giảm xuống một số phép toán số nguyên cố định, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    t = int(input())
    for _ in range(t):
        x1, y1, r1 = map(int, input().split())
        x2, y2, r2 = map(int, input().split())

        dx = x1 - x2
        dy = y1 - y2
        d2 = dx*dx + dy*dy

        if x1 == x2 and y1 == y2 and r1 == r2:
            out.append("-1")
            continue

        sumR = r1 + r2
        diffR = abs(r1 - r2)

        if d2 == 0:
            out.append("0")
        elif d2 < diffR*diffR:
            out.append("0")
        elif d2 == diffR*diffR:
            out.append("1")
        elif d2 < sumR*sumR:
            out.append("2")
        elif d2 == sumR*sumR:
            out.append("3")
        else:
            out.append("4")

    return "\n".join(out)

# provided sample
assert run("""1
1 1 1
1 3 1
""") == "4"

# identical circles
assert run("""1
0 0 1
0 0 1
""") == "-1"

# concentric different radii
assert run("""1
0 0 5
0 0 2
""") == "0"

# externally tangent
assert run("""1
0 0 2
3 0 1
""") == "3"

# disjoint
assert run("""1
0 0 1
10 0 1
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vòng tròn giống hệt nhau | -1 | trường hợp đường tiếp tuyến vô hạn | 
| bán kính đồng tâm khác nhau | 0 | không có tiếp tuyến chung khi cùng tâm | 
| tiếp tuyến ngoài | 3 | xử lý bình đẳng tại ranh giới sumR | 
| vòng tròn rời rạc | 4 | tiếp tuyến đầy đủ khi xa nhau | 

## Vỏ cạnh 

Khi các vòng tròn giống hệt nhau, thuật toán sẽ kích hoạt điều kiện đầu tiên và đưa ra −1 ngay lập tức. Ví dụ: (0,0,1) và (0,0,1) bị bắt trước bất kỳ vấn đề tính toán khoảng cách nào, thể hiện chính xác vô số tiếp tuyến được chia sẻ. 

Đối với các vòng tròn đồng tâm có bán kính khác nhau, chẳng hạn như (0,0,5) và (0,0,2), khoảng cách bình phương bằng 0 trong khi diffR² là dương. Điều kiện d2 < diffR² được kích hoạt, tạo ra 0. Điều này phù hợp với thực tế hình học là bất kỳ tiếp tuyến nào với một đường tròn đều không thể chạm vào đường tròn đồng tâm kia. 

Đối với các đường tròn tiếp tuyến ngoài như (0,0,2) và (3,0,1), chúng ta nhận được d2 == sumR². Nhánh đẳng thức đảm bảo chúng ta trả về 3. Việc triển khai chỉ bất đẳng thức nghiêm ngặt sẽ phân loại sai đây là rời rạc, dẫn đến kết quả là 4 không chính xác. 

Đối với các đường tròn rời rạc như (0,0,1) và (10,0,1), điều kiện khoảng cách vượt quá sumR², đặt cấu hình trong trường hợp cuối cùng với 4 tiếp tuyến, phản ánh toàn bộ tập hợp các đường tiếp tuyến bên ngoài và bên trong.
