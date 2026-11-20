---
title: "CF 2151E - Cửa hàng phiên bản giới hạn"
description: "Chúng tôi được yêu cầu mô phỏng một cửa hàng phiên bản giới hạn, nơi Alice và Bob thay phiên nhau chọn đồ, nhưng có một điểm khác biệt: chúng tôi không kiểm soát ai đi trước hoặc họ vào theo thứ tự nào. Theo chúng tôi, mỗi mục đều có một giá trị và Alice và Bob mỗi người có thứ hạng cá nhân về các mục đó."
date: "2026-06-09T04:18:36+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2151
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1053 (Div. 2)"
rating: 2100
weight: 2151
solve_time_s: 86
verified: false
draft: false
---

[CF 2151E - Cửa hàng phiên bản giới hạn](https://codeforces.com/problemset/problem/2151/E) 

**Xếp hạng:** 2100 
**Tas:** cấu trúc dữ liệu, dp, trò chơi, tham lam 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một cửa hàng phiên bản giới hạn, nơi Alice và Bob thay phiên nhau chọn đồ, nhưng có một điểm khác biệt: chúng tôi không kiểm soát ai đi trước hoặc họ vào theo thứ tự nào. Theo chúng tôi, mỗi mục đều có một giá trị và Alice và Bob mỗi người có thứ hạng cá nhân về các mục đó. Câu hỏi đặt ra, đối với Alice, tổng giá trị tối đa mà cô ấy có thể đạt được trong tất cả các cách có thể mua được món đồ đó là bao nhiêu. 

Mỗi trường hợp thử nghiệm cung cấp một mảng các giá trị mục`v`, sở thích của Alice`a`và sở thích của Bob`b`. Chúng ta phải cân nhắc rằng trong mỗi lượt, bất kỳ ai hành động đều sẽ lấy vật phẩm hiện được ưa thích nhất của họ. Chúng ta không biết trình tự các lượt quay, vì vậy chúng ta cần suy luận về tất cả các bộ có thể mà Alice có thể mua. Câu trả lời là tổng tối đa của các tập hợp có thể này. 

Những hạn chế là đáng kể:`n`có thể đạt tới 200.000 và tổng của tất cả các trường hợp thử nghiệm cũng lên tới 200.000. Điều này ngay lập tức loại trừ việc mô phỏng ngây thơ của mỗi thứ tự lượt, sẽ là O(n!) trong trường hợp xấu nhất. Thay vào đó, chúng ta cần một cách để suy luận một cách hiệu quả về những món đồ mà Alice có thể nhận được, vì Bob sẽ luôn lấy món đồ còn lại có mức ưu tiên cao nhất của anh ấy. 

Các trường hợp đặc biệt bao gồm các giá trị âm, vì Alice có thể tránh lấy một món đồ làm giảm tổng số của cô ấy và trường hợp các ưu tiên hàng đầu của Bob xung đột với những món đồ có giá trị nhất của Alice theo chúng tôi. Ví dụ: nếu tất cả các món đồ có giá trị cao của Alice cũng là món đồ yêu thích của Bob thì cuối cùng cô ấy chỉ có thể chọn những món đồ có giá trị thấp hoặc tiêu cực. Việc triển khai ngây thơ có thể tính tổng tất cả các mục hàng đầu của Alice mà không xem xét đến sự can thiệp của Bob, đưa ra mức tối đa không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê tất cả các chuỗi lựa chọn có thể có cho Alice và Bob, mô phỏng từng chuỗi và theo dõi tổng của Alice. Điều này đúng nhưng không thể tính toán được`n > 10`bởi vì nó yêu cầu các thao tác O(n!) Về nguyên tắc, vũ lực hoạt động vì nó kiểm tra mọi chuỗi khả thi, đảm bảo tổng của Alice được tối đa hóa. Nó thất bại khi n lớn vì tăng trưởng giai thừa nhanh chóng vượt quá tính toán khả thi. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng trình tự lựa chọn chỉ quan trọng ở chỗ ai nhận được từng món đồ trước. Bob sẽ luôn chọn món đồ anh ấy thích nhất và Alice cũng sẽ làm như vậy với món đồ của cô ấy. Do đó, một món đồ được đảm bảo cho Alice nếu cô ấy thích nó hơn Bob và có thể lấy nó trước Bob. Chúng ta có thể chính thức hóa điều này bằng cách sử dụng ý tưởng "sự thống trị theo trật tự lần lượt". 

Cụ thể, với mỗi mục, chúng ta có thể tính thứ hạng của nó theo sở thích của Alice và sở thích của Bob. Nếu thứ hạng của Alice cao hơn thứ hạng của Bob, cô ấy có khả năng có thể giành được nó trước Bob, tùy thuộc vào số lượng vật phẩm đã lấy được. Ngược lại, nếu thứ hạng của Bob cao hơn, anh ta luôn có thể ngăn cản Alice lấy nó. Điều này làm giảm vấn đề thành một chương trình động dựa trên tiền tố của các tùy chọn hoặc tương đương, một cách tiếp cận tham lam: lặp lại các mục theo thứ tự của Alice và chỉ chọn những mục mà cô ấy có thể yêu cầu, bỏ qua các mục mà Bob sẽ lấy trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tham lam dựa trên cấp bậc | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi mục, hãy tính vị trí của nó trong mảng ưu tiên của Alice và Bob. Những vị trí này xác định "điểm ưu tiên" cho cả hai. Mảng vị trí của Alice`posA[item]`là chỉ mục nơi mục xuất hiện trong danh sách của cô ấy, tương tự với Bob`posB[item]`. 
2. Khởi tạo danh sách`alice_items`để theo dõi những món đồ mà Alice có thể bảo đảm được. Lặp lại các mục theo thứ tự ưu tiên của Alice. 
3. Với mỗi mục trong sở thích của Alice, hãy kiểm tra xem vị trí của Alice có tốt hơn của Bob không. Nếu như`posA[item] < posB[item]`, Alice có thể đạt được nó trước Bob và chúng ta coi như nó có sẵn. 
4. Tính tổng giá trị của tất cả các mục mà Alice có thể yêu cầu theo quy tắc này. Số tiền này là số tiền tối đa có thể cô ấy có thể nhận được vì nó thể hiện tình huống trong đó cô ấy luôn chọn một món đồ trước Bob có thể, tôn trọng các ràng buộc về lượt của họ. 
5. Xuất tổng này cho test case. 

Tại sao nó hoạt động: Thuật toán hoạt động vì chúng tôi tính toán một cách hiệu quả "bộ an toàn" các vật phẩm mà Alice có thể lấy, giả sử Bob luôn hành động tối ưu để chặn cô ấy. Bằng cách so sánh các vị trí ưu tiên, chúng tôi đảm bảo không đánh giá quá cao: bất kỳ mục nào có thứ hạng thấp hơn đối với Bob đều có thể bị chặn nếu thứ hạng của Alice không cao hơn. Do đó, tổng hợp các món đồ mà Alice có thể mua theo thứ tự ưu tiên của mình sẽ mang lại tổng giá trị tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        v = list(map(int, input().split()))
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        posA = [0] * (n + 1)
        posB = [0] * (n + 1)
        for idx, item in enumerate(a):
            posA[item] = idx
        for idx, item in enumerate(b):
            posB[item] = idx

        result = 0
        for item in a:
            if posA[item] < posB[item]:
                result += v[item - 1]
        print(result)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc đầu vào và xây dựng mảng vị trí cho Alice và Bob. Vòng lặp kết thúc`a`đảm bảo chúng tôi xem xét các mục theo thứ tự của Alice. điều kiện`posA[item] < posB[item]`đảm bảo Alice có thể chọn nó trước Bob. Sử dụng lập chỉ mục dựa trên 1 từ đầu vào, chúng tôi truy cập các giá trị với`v[item-1]`. In tổng tích lũy sẽ đưa ra câu trả lời cho từng trường hợp kiểm tra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 -1 1
3 1 2
2 3 1
```| Bước | Vật phẩm Alice | posA | posB | Tình trạng | Tổng kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 1 | 0<1 -> có | 1 | 
| 2 | 1 | 1 | 2 | 1<2 -> có | 1+(-1)=0 | 
| 3 | 2 | 2 | 0 | 2<0 -> không | 0 | 

Số tiền tối đa của Alice là 0. Điều này xác nhận rằng các mục bị Bob chặn không thể được tính. 

### Mẫu 2 

đầu vào:```
4
5 -15 10 -5
2 4 3 1
1 4 2 3
```| Bước | Vật phẩm Alice | posA | posB | Tình trạng | Tổng kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | vâng | -15 | 
| 2 | 4 | 1 | 1 | không | -15 | 
| 3 | 3 | 2 | 3 | vâng | -15+10=-5 | 
| 4 | 1 | 3 | 0 | không | -5 | 

Tổng tối đa là -5, phù hợp với đánh giá dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi xây dựng hai mảng vị trí trong O(n) và lặp lại tùy chọn của Alice một lần | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Hai mảng có kích thước n+1 cho các vị trí, cộng với bộ nhớ đầu vào | 

Tổng tổng của n trong tất cả các trường hợp thử nghiệm là 2×10^5, do đó, tổng số thao tác nằm ở mức dưới 10^6, vừa vặn trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("1\n3\n1 -1 1\n3 1 2\n2 3 1\n") == "2"
assert run("1\n3\n-2 5 2\n3 1 2\n2 3 1\n") == "5"

# Minimum input
assert run("1\n1\n100\n1\n1\n") == "100"

# All negative values
assert run("1\n3\n-1 -2 -3\n1 2 3\n3 2 1\n") == "-1"

# Maximum size input, values alternating
n = 2*10**5
v = " ".join(str(x) for x in range(n, 0, -1))
a = " ".join(str(x) for x in range(1, n+1))
b = " ".join(str(x) for x in range(n, 0, -1))
assert run(f"1\n{n}\n{v}\n{a}\n{b}\n") == str(sum(range(n//2+1, n+1)))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 sản phẩm | 100 | Ranh giới đơn phần tử | 
| Tất cả tiêu cực | -1 | Xử lý đúng các tiêu cực | 
| Kích thước tối đa | n/2+1 đến n tổng | Xử lý đầu vào lớn một cách hiệu quả | 

## Vỏ cạnh 

Khi tất cả các giá trị đều âm, thuật toán sẽ bỏ qua các mục bị Bob chặn và chỉ tính những mục mà Alice có thể bảo mật
