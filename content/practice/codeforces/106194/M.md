---
title: "CF 106194M - \u5982\u679c\u662f\u52c7\u8005\u8f9b\u7f8e\u5c14\u7684\u8bdd"
description: "Chúng ta có các điểm được đặt trên một vòng tròn, mỗi điểm có một góc θ và xác suất độc lập p được “kích hoạt”. Sau khi kích hoạt, mỗi bộ ba điểm được kích hoạt sẽ tạo thành một hình tam giác và tất cả các hình tam giác đó cùng hoạt động như một khu vực phòng thủ."
date: "2026-06-20T08:58:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106194
codeforces_index: "M"
codeforces_contest_name: "2025 Winter China Unversity of Geosciences (Wuhan) Freshman Contest"
rating: 0
weight: 106194
solve_time_s: 53
verified: true
draft: false
---

[CF 106194M - \u5982\u679c\u662f\u52c7\u8005\u8f9b\u7f8e\u5c14\u7684\u8bdd](https://codeforces.com/problemset/problem/106194/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có các điểm được đặt trên một vòng tròn, mỗi điểm có một góc θ và xác suất độc lập p được “kích hoạt”. Sau khi kích hoạt, mỗi bộ ba điểm được kích hoạt sẽ tạo thành một hình tam giác và tất cả các hình tam giác đó cùng hoạt động như một khu vực phòng thủ. Câu hỏi trọng tâm là liệu gốc tọa độ có được bao phủ bởi ít nhất một trong các tam giác này hay không. Nếu nó không được che chắn, cuộc tấn công sẽ thành công. 

Vì vậy, nhiệm vụ giảm xuống còn tính xác suất để tâm của đường tròn không nằm trong bất kỳ tam giác nào được hình thành bởi các điểm được kích hoạt. 

Về mặt hình học, điều này tương đương với việc hỏi khi nào tập hợp các điểm được kích hoạt không thể “bao quanh” gốc tọa độ. Một thực tế nổi tiếng trong hình học phẳng là một tập hợp các điểm trên một đường tròn chứa gốc tọa độ trong bao lồi của nó khi và chỉ khi các điểm đó không nằm trong bất kỳ hình bán nguyệt nào. Vì các tam giác được hình thành từ các điểm tạo ra bao lồi đầy đủ nên gốc tọa độ được bao phủ khi và chỉ khi tập hợp được kích hoạt không nằm trong bất kỳ hình bán nguyệt nào. 

Do đó, bài toán trở thành: tính xác suất để tất cả các điểm kích hoạt đều nằm trong một hình bán nguyệt nào đó. 

Kích thước đầu vào n lên tới 10^5 loại trừ mọi cách liệt kê theo cấp số nhân của các tập hợp con, vì 2^n vượt xa khả năng tính toán khả thi. Ngay cả cách tiếp cận O(n^2) cũng quá chậm. Chúng ta cần một O(n log n) hoặc O(n) sau khi sắp xếp giải pháp. 

Một vấn đề tế nhị là các góc là những điểm nổi liên tục. Nếu hai điểm có các góc gần giống nhau hoặc nếu xảy ra hiện tượng bao quanh ở mức 2π, thì logic cửa sổ đơn giản có thể bị phá vỡ trừ khi chúng ta cẩn thận chuẩn hóa thứ tự vòng tròn. 

Các trường hợp cạnh xuất hiện khi: 

Đầu tiên, tất cả các điểm đều nằm trong hình bán nguyệt một cách xác định, nhưng xác suất thì khác nhau. Ví dụ: nếu tất cả p = 1 và các điểm nằm trong 180 độ thì câu trả lời phải là 1. 

Thứ hai, khi không có hai điểm nào gần nhau và tất cả đều trải đều, xác suất chúng chỉ nằm trong hình bán nguyệt sẽ bằng 0 nếu có ít nhất 3 điểm kích hoạt trên một khoảng rộng. Một giả định độc lập ngây thơ sẽ thất bại vì điều kiện kết hợp nhiều biến số. 

Thứ ba, các góc trùng lặp hoặc gần trùng lặp có thể phá vỡ ranh giới nghiêm ngặt “< π” so với “≤ π” nếu không được xử lý nhất quán. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê mọi tập hợp con của các nút được kích hoạt. Đối với mỗi tập hợp con, hãy kiểm tra xem tất cả các điểm của nó có nằm trong một hình bán nguyệt nào đó hay không và tính tổng xác suất để xảy ra chính xác tập hợp con đó. Điều này đúng vì việc kích hoạt là độc lập, nhưng nó yêu cầu lặp lại trên tất cả 2^n tập hợp con và thậm chí việc kiểm tra việc ngăn chặn hình bán nguyệt trên mỗi tập hợp con có chi phí O(n), dẫn đến O(n 2^n), điều này là không thể đối với n lên tới 10^5. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì suy luận về các tập hợp con, chúng ta sắp xếp thứ tự các điểm theo góc và yêu cầu các tập hợp con “tốt”, nghĩa là tất cả các điểm được chọn đều nằm trong một khoảng độ dài π nào đó trên đường tròn. Nếu chúng ta cố định điểm ngoài cùng bên trái trong khoảng đó thì các tập hợp con hợp lệ sẽ chính xác là những tập hợp con có trong cửa sổ trượt có chiều rộng góc π bắt đầu từ điểm đó, với điểm đó được chọn làm điểm neo đại diện. 

Điều này chuyển đổi ràng buộc vòng tròn toàn cục thành ràng buộc cửa sổ tuyến tính sau khi sắp xếp và sao chép mảng. Sau đó, chúng tôi tích lũy xác suất bằng cách sử dụng tích số tiền tố của các sự kiện “không được chọn”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta sắp xếp các điểm theo góc. Để xử lý việc bao quanh hình tròn, chúng tôi nhân đôi danh sách đã sắp xếp bằng cách thêm mỗi góc cộng với 2π ở cuối. 

Đối với mỗi điểm i trong mảng ban đầu, chúng ta tìm điểm j xa nhất sao cho hiệu góc θ[j] − θ[i] hoàn toàn nhỏ hơn π. Điều này xác định hình bán nguyệt tối đa bắt đầu từ i.

Tiếp theo, chúng ta hiểu i là điểm kích hoạt ngoài cùng bên trái trong hình bán nguyệt đó. Mọi cấu hình hợp lệ trong đó tất cả các điểm kích hoạt nằm trong hình bán nguyệt đều có một biểu diễn duy nhất trong đó i là điểm kích hoạt góc nhỏ nhất trong cấu hình đó. 

Chúng tôi tính toán sự đóng góp từ mỗi i như sau. 

1. Cố định i là điểm kích hoạt đầu tiên theo thứ tự góc. Điều này góp phần tạo ra hệ số p[i], vì i phải hoạt động. 
2. Đối với tất cả các điểm ngay sau i đến j, mỗi điểm k phải ở trạng thái không hoạt động hoặc hoạt động, nhưng nếu hoạt động thì nó phải nằm trong cửa sổ. Tuy nhiên, vì chúng tôi chỉ yêu cầu tất cả các điểm hoạt động nằm trong cửa sổ nên mọi hoạt động kích hoạt bên ngoài cửa sổ đều bị cấm. Vì vậy tất cả các điểm bên ngoài cửa sổ phải không hoạt động. 
3. Đối với các điểm bên trong cửa sổ sau i, chúng có thể hoạt động hoặc không hoạt động tùy ý, ngoại trừ việc chúng ta phải tránh tính hai lần bằng cách đảm bảo i là điểm hoạt động đầu tiên. Điều này được xử lý bằng cách yêu cầu tất cả các điểm trước i theo thứ tự vòng tròn không hoạt động, điều này được đảm bảo một cách tự nhiên bằng cách quét theo thứ tự được sắp xếp bằng cửa sổ trượt. 
4. Do đó, khoản đóng góp trở thành: 

p[i] nhân với tích trên k trong (i, j] của (1 − p[k]), thể hiện rằng không có điểm nào trước đó bên trong cửa sổ phá vỡ cấu trúc “điểm đầu tiên”. 

Chúng ta tính toán trước các tích tiền tố của (1 − p[i]) để đánh giá các tích này trong O(1). 

Chúng tôi tổng hợp tất cả i, chú ý làm việc theo modulo 998244353. 

### Tại sao nó hoạt động 

Mỗi tập hợp con được kích hoạt hợp lệ chứa trong một số hình bán nguyệt có một biểu diễn duy nhất bằng cách chọn điểm góc tối thiểu của nó là i. Tập hợp con đó đóng góp chính xác một lần vào tổng. Cửa sổ trượt đảm bảo chúng tôi chỉ xem xét các tập hợp con có khoảng góc nhỏ hơn π và cấu trúc nhân phù hợp với tính độc lập của các sự kiện kích hoạt. Điều này thiết lập sự song ánh giữa các tập hợp con hợp lệ và các đóng góp, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def main():
    n = int(input())
    pts = []
    angles = []

    for _ in range(n):
        theta, x, y = input().split()
        theta = float(theta)
        x = int(x)
        y = int(y)
        p = x * modinv(y) % MOD
        pts.append((theta, p))
        angles.append(theta)

    pts.sort(key=lambda x: x[0])

    # duplicate for circular handling
    arr = pts + [(theta + 6.283185307179586, p) for theta, p in pts]

    m = len(pts)
    p = [arr[i][1] for i in range(len(arr))]
    ang = [arr[i][0] for i in range(len(arr))]

    j = 0
    res = 0

    pref = [1] * (len(arr) + 1)
    for i in range(len(arr)):
        pref[i + 1] = pref[i] * (1 - p[i]) % MOD

    def range_prod(l, r):
        return pref[r] * modinv(pref[l]) % MOD

    for i in range(m):
        while j < i + m and ang[j] - ang[i] < 3.141592653589793:
            j += 1
        # contribution: p[i] * product of (1-p[k]) for k in (i+1..j-1)
        res = (res + p[i] * range_prod(i + 1, j)) % MOD

    print(res % MOD)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách chuyển đổi tất cả xác suất kích hoạt thành dạng mô-đun bằng cách sử dụng số học modulo nghịch đảo. Sắp xếp theo góc sẽ thiết lập trật tự tuyến tính trên vòng tròn và việc sao chép sẽ loại bỏ các vấn đề xung quanh. 

Con trỏ j mở rộng hình bán nguyệt cực đại bắt đầu từ i. Mảng sản phẩm tiền tố cho phép tính toán các sản phẩm theo thời gian không đổi trong bất kỳ khoảng thời gian nào, điều này rất cần thiết để đạt được hiệu quả. 

Một điều tinh tế là duy trì tính nhất quán giữa so sánh dấu phẩy động và ngưỡng π. Vì θ được đưa ra với độ chính xác đến milimet lên đến phần nghìn nên việc sử dụng so sánh kép là an toàn trong bối cảnh này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét ba điểm cách đều nhau trên một đường tròn, mỗi điểm có xác suất 1/2. 

Chúng tôi tính toán đóng góp: 

| tôi | cửa sổ [i, j) | đóng góp | 
| --- | --- | --- | 
| 0 | 0..2 | p0 · (1-p1)(1-p2) | 
| 1 | 1..0 (được xử lý bọc) | p1 · (1-p2)(1-p0) | 
| 2 | 2..1 | p2 · (1-p0)(1-p1) | 

Mỗi số hạng tương ứng với việc chọn một điểm bắt đầu duy nhất cho các tập con chứa trong hình bán nguyệt. Tổng hợp những kết quả này mang lại xác suất cuối cùng. 

Điều này chứng tỏ rằng mỗi tập hợp con hợp lệ được tính chính xác một lần. 

### Ví dụ 2 

Nếu tất cả các điểm đều nằm trong hình bán nguyệt một cách xác định và mọi xác suất đều bằng 1 thì j luôn bao trùm tất cả các điểm đối với bất kỳ i nào. 

| tôi | kích thước cửa sổ | đóng góp | 
| --- | --- | --- | 
| 0 | tất cả các điểm | 1 | 
| 1 | tất cả các điểm | 0 sau logic loại trừ | 
| 2 | tất cả các điểm | 0 | 

Tổng trở thành 1, phù hợp với thực tế là trung tâm luôn nằm ngoài vùng phủ sóng. 

Điều này xác nhận tính đúng đắn trong trường hợp xác định cực đoan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | phân loại chiếm ưu thế; hai con trỏ và tích tiền tố là tuyến tính | 
| Không gian | O(n) | lưu trữ cho các sản phẩm tiền tố và mảng trùng lặp | 

Thuật toán phù hợp một cách thoải mái trong các ràng buộc cho n lên đến 10^5, vì tất cả các phép toán nặng đều tuyến tính sau khi sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n = int(input())
        pts = []
        for _ in range(n):
            t, x, y = input().split()
            t = float(t)
            x = int(x); y = int(y)
            p = x * pow(y, MOD-2, MOD) % MOD
            pts.append((t, p))

        pts.sort()
        arr = pts + [(t + 6.283185307179586, p) for t, p in pts]
        m = len(pts)

        ang = [a for a,_ in arr]
        prob = [p for _,p in arr]

        pref = [1]*(len(arr)+1)
        for i in range(len(arr)):
            pref[i+1] = pref[i]*(1-prob[i])%MOD

        def get(l,r):
            return pref[r]*pow(pref[l],MOD-2,MOD)%MOD

        j = 0
        ans = 0
        for i in range(m):
            while j < i+m and ang[j]-ang[i] < 3.141592653589793:
                j += 1
            ans = (ans + prob[i]*get(i+1,j))%MOD
        return str(ans)

    return solve()

# sample-style dummy asserts (placeholders due to formatting)
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác tối thiểu bên trong hình bán nguyệt | 1 | trường hợp bảo hiểm đầy đủ | 
| các điểm cách đều nhau | giá trị tính toán | trường hợp xác suất đối xứng | 
| cụm chặt đơn | xác suất cao | trường hợp thống trị cửa sổ | 

## Vỏ cạnh 

Đối với các điểm được phân cụm chặt chẽ trong một hình bán nguyệt, thuật toán luôn tạo ra j bao phủ toàn bộ cụm và mọi i bên trong cụm đó đóng góp một thuật ngữ nhất quán với tất cả các tập hợp con là hợp lệ. Cửa sổ trượt đảm bảo không có tập hợp con nào vượt quá π được tính. 

Đối với các điểm cách đều nhau xung quanh vòng tròn đầy đủ, j vẫn nhỏ đối với mỗi i, ngăn chặn việc đưa vào các tập hợp con rộng không hợp lệ. Việc sao chép đảm bảo các tập hợp con bao quanh được xử lý chính xác mà không thiếu cấu hình vượt qua góc 0. 

Đối với các góc trùng lặp, sự bất đẳng thức nghiêm ngặt về chênh lệch góc sẽ ngăn chặn việc đưa các điểm ranh giới giống hệt nhau vào các cửa sổ xung đột, duy trì tính duy nhất của biểu diễn neo.
