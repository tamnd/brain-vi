---
title: "CF 1380B - Giải pháp phổ quát"
description: "Chúng ta được cung cấp một chuỗi mô tả chiến lược đối thủ theo chu kỳ trong trò chơi oẳn tù tì. Đối thủ không thích ứng; thay vào đó, họ chọn nước đi theo một chuỗi tròn cố định. Nếu chúng bắt đầu ở một vị trí nào đó, chúng sẽ đi theo chuỗi theo thứ tự và quấn quanh mãi mãi."
date: "2026-06-16T13:39:17+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1380
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 91 (Rated for Div. 2)"
rating: 1400
weight: 1380
solve_time_s: 304
verified: false
draft: false
---

[CF 1380B - Giải pháp phổ quát](https://codeforces.com/problemset/problem/1380/B) 

**Đánh giá:** 1400 
**Thẻ:** tham lam 
**Thời gian giải:** 5 phút 4 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi mô tả chiến lược đối thủ theo chu kỳ trong trò chơi oẳn tù tì. Đối thủ không thích ứng; thay vào đó, họ chọn nước đi theo một chuỗi tròn cố định. Nếu chúng bắt đầu ở một vị trí nào đó, chúng sẽ đi theo chuỗi theo thứ tự và quấn quanh mãi mãi. 

Chúng ta phải xây dựng chuỗi bước đi dài n của riêng mình. Điều đáng chú ý là chúng ta không biết vị trí xuất phát của đối thủ. Đối với mỗi lượt xuất phát có thể có của đối thủ, chúng tôi chơi n hiệp và đếm xem nước đi của chúng tôi đánh bại họ bao nhiêu lần. Mỗi ca tạo ra số trận thắng khác nhau và chúng tôi quan tâm đến mức trung bình của số trận thắng này trong tất cả các ca. 

Vì vậy, nhiệm vụ không phải là tối đa hóa hiệu suất dựa trên một sự liên kết duy nhất. Thay vào đó, chúng tôi đang tối ưu hóa tổng số trận thắng tổng hợp trên tất cả các sắp xếp theo chu kỳ của chuỗi đối thủ. 

Ràng buộc n lên tới 200000 trong tất cả các trường hợp thử nghiệm ngụ ý rằng chúng ta cần xây dựng tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm. Bất kỳ cách tiếp cận nào mô phỏng tất cả các ca một cách rõ ràng sẽ yêu cầu O(n^2), tốc độ này quá chậm vì sẽ đạt khoảng 4e10 thao tác trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi đối thủ đồng nhất. Ví dụ: nếu chuỗi là "RRRR", thì mọi vị trí đều tương đương. Một chiến lược tham lam ngây thơ thay đổi bước đi của chúng ta cho mỗi vị trí có vẻ hợp lý, nhưng nó không giúp ích gì, bởi vì mọi vị trí đều đối xứng trong tất cả các ca, do đó chỉ có tần số tổng hợp mới quan trọng. 

Một trường hợp góc khác là khi dây cân bằng như "RSP". Ở đây, các lựa chọn di chuyển khác nhau của chúng tôi ảnh hưởng đến các ca khác nhau một cách không đồng đều, nhưng một lần nữa, điều quan trọng là tổng đóng góp cho tất cả các lần luân chuyển chứ không phải tối ưu hóa mỗi ca. 

## Phương pháp tiếp cận 

Một công trình mạnh mẽ sẽ thử mọi chuỗi hành động có thể có của chúng ta. Đối với mỗi ứng cử viên, nó mô phỏng tất cả n lần dịch chuyển theo chu kỳ của chuỗi đối thủ, tính toán số lần thắng và tính trung bình cho chúng. Mỗi mô phỏng có chi phí O(n) và có nhiều chuỗi ứng cử viên theo cấp số nhân, khiến điều này hoàn toàn không khả thi. 

Chúng tôi có thể giảm đáng kể điều này bằng cách quan sát xem mỗi vị trí trong chuỗi của chúng tôi đóng góp như thế nào vào điểm tổng thể. Sửa vị trí i trong chuỗi của chúng tôi. Khi sự thay đổi của đối thủ thay đổi, ký tự được căn chỉnh theo vị trí i sẽ duyệt qua mọi ký tự của chuỗi đối thủ chính xác một lần. Điều này có nghĩa là sự đóng góp của vị trí i chỉ phụ thuộc vào số lượng ký tự đối thủ mà nó có thể đánh bại trong toàn bộ chuỗi chứ không phụ thuộc vào việc căn chỉnh theo vị trí cụ thể. 

Điều này giải quyết vấn đề: mỗi vị trí trong câu trả lời của chúng tôi có thể được chọn độc lập và chúng tôi chỉ cần tối đa hóa số lượng ký tự trong chuỗi đối thủ mà nó đánh bại. Vì tất cả các vị trí đều hoạt động giống hệt nhau dưới sự dịch chuyển theo chu kỳ nên lựa chọn tối ưu là như nhau cho mọi vị trí. 

Vì vậy, vấn đề giảm xuống việc tính toán các tần số R, S và P trong chuỗi của đối thủ và chọn nước đi đánh bại mục tiêu thường xuyên nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng chính 

Chúng tôi chọn một nước đi duy nhất cho tất cả các vị trí trong câu trả lời của mình, dựa trên nước đi nào mang lại tổng số chiến thắng nhiều nhất trên chuỗi đối thủ. 

### Các bước 

1. Đếm số lần mỗi ký tự R, S, P xuất hiện trong chuỗi đối thủ. 

Mục đích là để đánh giá tổng hiệu quả của từng nước đi có thể có so với sự phân bố đầy đủ các nước đi của đối thủ. 
2. Tính số phần thắng mà mỗi nước đi có thể mang lại: 

Nếu chúng ta chơi R, chúng ta thắng S. 

Nếu chúng ta chơi S, chúng ta thắng P. 

Nếu chúng ta chơi P, chúng ta thắng R. 

Mỗi tần số này tương ứng với một tần số cụ thể trong chuỗi đối thủ. 
3. So sánh ba giá trị: 

won(R) = số lượng S trong chuỗi, 

thắng(S) = số P,

thắng(P) = số lượng R. 
4. Chọn nước đi có giá trị thắng lớn nhất. Nếu hòa, bất kỳ nước đi hòa nào đều hợp lệ. 
5. Đầu ra di chuyển lặp lại n lần để tạo thành câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Đối với bất kỳ vị trí cố định nào trong chuỗi của chúng ta, trên tất cả các dịch chuyển theo chu kỳ của chuỗi đối thủ, nó tương tác một lần với mọi ký tự trong chuỗi đối thủ. Do đó, tổng đóng góp của nó chỉ phụ thuộc vào tần số chung của các biểu tượng đối thủ chứ không phụ thuộc vào thứ tự của chúng. Vì mọi vị trí đều có hành vi giống nhau dưới mức trung bình qua các ca, nên việc chọn cùng một nước đi tối ưu ở mọi nơi sẽ duy trì tính tối ưu và tối đa hóa tổng số tiền thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    cntR = cntS = cntP = 0
    for ch in s:
        if ch == 'R':
            cntR += 1
        elif ch == 'S':
            cntS += 1
        else:
            cntP += 1

    # wins if we choose each move
    winR = cntS
    winS = cntP
    winP = cntR

    if winR >= winS and winR >= winP:
        best = 'R'
    elif winS >= winP:
        best = 'S'
    else:
        best = 'P'

    print(best * n)

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Bước đếm sẽ tách chuỗi thành ba bộ tích lũy, đây là thông tin duy nhất mà đối thủ cần. Việc so sánh trực tiếp đánh giá ba chiến lược có thể. 

Chi tiết triển khai chính là các mối quan hệ phải được xử lý một cách nhất quán. Bất kỳ tie-break nào cũng đúng, do đó mã chỉ ưu tiên R, sau đó là S, rồi P theo thứ tự các điều kiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`RSP`Chúng tôi tính toán tần số: R = 1, S = 1, P = 1. 

| Ứng viên chuyển đi | Nhịp đập | Tổng số trận thắng | 
| --- | --- | --- | 
| R | S | 1 | 
| S | P | 1 | 
| P | R | 1 | 

Tất cả các bước di chuyển đều tương đương nhau, vì vậy bất kỳ chuỗi thống nhất nào cũng là tối ưu. Giả sử chúng ta chọn "R". 

Đối với mỗi vòng quay, mỗi vị trí nhìn thấy mỗi nhân vật của đối thủ chính xác một lần, do đó, mỗi lần thay đổi mang lại chính xác một chiến thắng trong mỗi chu kỳ đầy đủ, tạo ra hành vi đồng nhất. 

### Ví dụ 2:`RRRR`Tần số: R = 4, S = 0, P = 0. 

| Ứng viên chuyển đi | Nhịp đập | Tổng số trận thắng | 
| --- | --- | --- | 
| R | S | 0 | 
| S | P | 0 | 
| P | R | 4 | 

Nước đi tốt nhất là P, vì vậy đầu ra là "PPPP". 

Điều này xác nhận rằng mặc dù đối thủ không bao giờ chơi S hoặc P, việc chọn P sẽ khai thác thực tế là P đánh bại R ở mọi hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm quét chuỗi một lần để đếm tần số | 
| Không gian | O(1) | Chỉ có ba bộ đếm được sử dụng bất kể kích thước đầu vào | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi 2e5, do đó, việc quét tuyến tính trên mỗi trường hợp thử nghiệm có thể dễ dàng đủ nhanh trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        s = input().strip()
        cntR = cntS = cntP = 0
        for ch in s:
            if ch == 'R':
                cntR += 1
            elif ch == 'S':
                cntS += 1
            else:
                cntP += 1

        winR = cntS
        winS = cntP
        winP = cntR

        if winR >= winS and winR >= winP:
            best = 'R'
        elif winS >= winP:
            best = 'S'
        else:
            best = 'P'

        return best * len(s)

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        s = sys.stdin.readline().strip()
        cntR = s.count('R')
        cntS = s.count('S')
        cntP = s.count('P')

        winR = cntS
        winS = cntP
        winP = cntR

        if winR >= winS and winR >= winP:
            out.append('R' * len(s))
        elif winS >= winP:
            out.append('S' * len(s))
        else:
            out.append('P' * len(s))

    return "\n".join(out) + "\n"

# provided samples
assert run("3\nRRRR\nRSP\nS\n") == "PPPP\nRSP\nR\n"

# all same character
assert run("1\nSSSSS\n") == "PPPPP\n"

# skewed distribution
assert run("1\nRRSS") == "PPPP\n"

# single char
assert run("1\nR\n") == "P\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| RRRRR | PPPPP | Tối đa hóa chiến thắng trước đối thủ đồng đều | 
| RSP | lựa chọn đồng phục tối ưu nào | Xử lý phân phối cân bằng | 
| RRSS | PPPP | Độ chính xác sai lệch tần số | 
| S | R | Độ chính xác đầu vào tối thiểu | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như “R”, đối thủ luôn chơi R trong mỗi ca. Thuật toán đếm R = 1 và chọn P vì P đánh bại R. Đầu ra trở thành "P", mang lại chiến thắng được đảm bảo trong mọi đánh giá. 

Đối với các chuỗi đồng nhất như "RRRRRR", mọi ca đều giống hệt nhau. Thuật toán xác định chính xác chỉ di chuyển vật chất R nên chọn P cho tất cả các vị trí. Vì mọi căn chỉnh đều giống hệt nhau nên lựa chọn này sẽ tối đa hóa tất cả số lần thắng cùng một lúc. 

Đối với các chuỗi có độ hỗn hợp cao như "RSPRSP", tất cả các tần số đều bằng nhau. Bất kỳ R, S hoặc P nào cũng là tối ưu và quy tắc ràng buộc sẽ tạo ra một câu trả lời thống nhất hợp lệ.
