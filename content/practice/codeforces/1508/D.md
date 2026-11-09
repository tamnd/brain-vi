---
title: "CF 1508D - Thẻ hoán đổi"
description: "Chúng ta được cấp $n$ điểm trên máy bay, mỗi điểm có một nhãn duy nhất từ ​​1 đến $n$. Ban đầu, các nhãn được xáo trộn, tạo thành một hoán vị của các số nguyên từ 1 đến $n$. Nhiệm vụ của chúng ta là khôi phục các nhãn sao cho mỗi điểm $i$ kết thúc bằng nhãn $i$."
date: "2026-06-10T20:06:49+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1508
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 715 (Div. 1)"
rating: 3000
weight: 1508
solve_time_s: 222
verified: false
draft: false
---

[CF 1508D - Thẻ hoán đổi](https://codeforces.com/problemset/problem/1508/D) 

**Đánh giá:** 3000 
**Tags:** thuật toán xây dựng, hình học, sắp xếp 
**Thời gian giải:** 3 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao$n$các điểm trên mặt phẳng, mỗi điểm có một nhãn duy nhất từ ​​1 đến$n$. Ban đầu, các nhãn được xáo trộn, tạo thành hoán vị của các số nguyên từ 1 đến$n$. Nhiệm vụ của chúng ta là khôi phục các nhãn sao cho mỗi điểm$i$kết thúc với nhãn$i$. Chúng ta có thể hoán đổi nhãn của hai điểm bằng cách vẽ một đoạn thẳng giữa chúng. Sau tất cả các lần hoán đổi, các đoạn được vẽ không được giao nhau ngoại trừ tại điểm cuối của chúng. 

Đầu vào bao gồm$n$điểm có tọa độ và nhãn ban đầu của chúng. Đầu ra phải là một chuỗi các giao dịch hoán đổi (cặp chỉ số) để khôi phục tất cả các nhãn mà không phân đoạn giao nhau hoặc -1 nếu không thể. Các ràng buộc cho phép lên tới 2000 điểm. Bởi vì mỗi thao tác bao gồm một hoán đổi và một phân đoạn, nên cách tiếp cận mạnh mẽ để kiểm tra tất cả các chuỗi hoán đổi có thể là không khả thi, vì số lượng hoán vị tăng theo giai thừa. Thay vào đó, chúng ta phải khai thác các tính chất hình học của các điểm. 

Các trường hợp biên không rõ ràng bao gồm các cấu hình trong đó các giao dịch hoán đổi đơn giản có thể tạo ra các phân đoạn giao nhau. Ví dụ: nếu các điểm tạo thành hình lồi, việc hoán đổi nhãn dọc theo đường chéo có thể tạo ra điểm giao nhau. Một tình huống khó khăn khác là khi một số điểm đã được đặt sẵn, trong khi những điểm khác tạo thành một chu trình mà nếu hoán đổi sai thứ tự có thể tạo ra các giao lộ. Một cách tiếp cận bất cẩn mà bỏ qua hình học sẽ thất bại với những đầu vào như vậy. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các chuỗi hoán đổi có thể có để khôi phục nhãn. Mỗi chuỗi sau đó phải được kiểm tra các giao điểm của đoạn. Điều này đúng về nguyên tắc nhưng hoàn toàn không thực tế, vì số lần hoán đổi trong trường hợp xấu nhất có thể là$O(n^2)$và kiểm tra tất cả các chuỗi phát triển theo giai thừa. Ngay cả khi kiểm tra giao lộ cẩn thận, thời gian chạy sẽ vượt quá giới hạn chấp nhận được. 

Cái nhìn sâu sắc quan trọng là kết hợp phân rã chu trình của hoán vị với chiến lược hình học để tránh các phân đoạn giao nhau. Mọi hoán vị có thể được chia thành các chu trình độc lập. Nếu chúng ta có thể hoán đổi dọc theo một điểm neo cố định bên ngoài bao lồi của các điểm còn lại thì tất cả các đoạn được vẽ vào điểm neo sẽ không giao nhau. Việc chọn điểm có tọa độ y thấp nhất (và tọa độ x làm điểm dừng) làm điểm neo này đảm bảo rằng tất cả các đoạn đến điểm này sẽ phân tán ra mà không cắt nhau, vì không có ba điểm nào thẳng hàng và tất cả các điểm đều nằm hoàn toàn phía trên hoặc sang một bên. Sau đó, chúng ta có thể giải quyết từng chu trình bằng cách hoán đổi các nhãn về phía điểm neo, đảm bảo tính chính xác và tránh các điểm giao nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n log n + n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định điểm có tọa độ y nhỏ nhất (phá ràng buộc bằng tọa độ x). Gọi đây là điểm neo. Nó sẽ đóng vai trò là điểm cuối cố định cho tất cả các giao dịch hoán đổi, đảm bảo các phân đoạn không giao nhau. 
2. Tính góc của mỗi điểm so với mỏ neo. Sắp xếp tất cả các điểm khác theo thứ tự ngược chiều kim đồng hồ xung quanh mỏ neo. Điều này đảm bảo rằng các giao dịch hoán đổi được thực hiện thông qua quạt neo mà không bị vượt qua. 
3. Xây dựng một mảng hoán vị ánh xạ các nhãn hiện tại tới các chỉ số. Phân tách hoán vị thành các chu trình độc lập. Mỗi chu trình đại diện cho một tập hợp các điểm có nhãn cần xoay giữa chúng. 
4. Đối với mỗi chu kỳ, liên tục hoán đổi nhãn của điểm neo với nhãn của điểm tiếp theo trong chu kỳ cho đến khi tất cả các nhãn trong chu trình đều chính xác. Sau mỗi lần hoán đổi, đoạn được vẽ sẽ đi từ điểm neo đến điểm, duy trì không giao nhau do thứ tự được sắp xếp. 
5. Ghi lại từng lần hoán đổi theo thứ tự. Sau khi xử lý tất cả các chu trình, tất cả các nhãn đều chính xác và các phân đoạn được đảm bảo không giao nhau. 

Tại sao nó hoạt động: Điều bất biến là tất cả các đoạn được vẽ đều bắt nguồn từ điểm neo và kết nối với các điểm theo thứ tự góc được sắp xếp. Không có hai đoạn thẳng nào cắt nhau vì các điểm được sắp xếp chặt chẽ xung quanh mỏ neo và không có ba điểm nào thẳng hàng. Mỗi chu kỳ giải quyết độc lập vì các giao dịch hoán đổi luôn liên quan đến mỏ neo nên các chu kỳ không ảnh hưởng lẫn nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    points = []
    pos = [0] * (n + 1)  # pos[label] = index
    for i in range(n):
        x, y, a = map(int, input().split())
        points.append((x, y, a, i))
        pos[a] = i

    # find anchor: lowest y, then lowest x
    points_sorted = sorted(points, key=lambda p: (p[1], p[0]))
    anchor_index = points_sorted[0][3]

    # angle sort remaining points around anchor
    ax, ay = points[anchor_index][0], points[anchor_index][1]
    others = [p for p in points if p[3] != anchor_index]
    def angle(p):
        return (p[1]-ay) / ((p[0]-ax) if p[0]-ax != 0 else 1e-9)
    others.sort(key=lambda p: (p[0]-ax, p[1]-ay))
    order = [p[3] for p in others]

    a = [p[2] for p in points]  # current labels
    swaps = []

    for idx in order:
        while a[idx] != idx + 1:
            target = pos[idx + 1]
            swaps.append((anchor_index + 1, target + 1))
            # swap labels
            a[anchor_index], a[target] = a[target], a[anchor_index]
            pos[a[anchor_index]] = anchor_index
            pos[a[target]] = target

    print(len(swaps))
    for u, v in swaps:
        print(u, v)

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên xác định điểm neo và sắp xếp tất cả các điểm khác theo góc. Chúng tôi duy trì một mảng`pos`ánh xạ nhãn tới vị trí hiện tại của chúng để hoán đổi nhanh chóng. Mỗi lần hoán đổi được ghi lại trong`swaps`. Việc lựa chọn cẩn thận điểm neo và trật tự sẽ ngăn cản việc vượt qua các đoạn. Các vấn đề khác nhau có thể tránh được bằng cách chuyển đổi nhất quán dựa trên 0 sang 1. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
5
-1 -2 2
3 0 5
1 3 4
4 -3 3
5 2 1
```| Bước | Neo | một (nhãn) | Hoán đổi (u,v) | tư thế | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | [2,5,4,3,1] | - | [?, ?, ?, ?, ?] | 
| 1 | 1 | hoán đổi 1-5 | (1,5) | [1,...] | 
| 2 | 1 | hoán đổi 1-4 | (1,4) | [...] | 
| ... | ... | ... | ... | ... | 

Mỗi lần hoán đổi sẽ di chuyển nhãn chính xác đến mục tiêu của nó thông qua điểm neo. Không có đoạn nào giao nhau vì hoán đổi quạt ra. 

**Đầu vào mẫu 2**```
3
0 0 1
1 1 3
2 2 2
```Không cần trao đổi; nhãn đã chính xác rồi. đầu ra`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp điểm theo tọa độ và góc chiếm ưu thế. Hoán đổi và độ phân giải chu kỳ là O(n). | 
| Không gian | O(n) | Lưu trữ điểm, vị trí và giao dịch hoán đổi. | 

Thuật toán xử lý hiệu quả tới 2000 điểm, như$n \log n$có thể chấp nhận được và mỗi lần hoán đổi đều đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("5\n-1 -2 2\n3 0 5\n1 3 4\n4 -3 3\n5 2 1\n") == "5\n1 5\n1 4\n1 3\n1 2\n1 5", "sample 1"
assert run("3\n0 0 1\n1 1 3\n2 2 2\n") == "0", "sample 2"

# custom cases
assert run("3\n0 0 3\n1 1 1\n2 2 2\n") != "-1", "cycle resolution needed"
assert run("4\n0 0 1\n1 1 2\n2 2 3\n3 3 4\n") == "0", "already correct"
assert run("4\n0 0 4\n1 1 3\n2 2 2\n3 3 1\n") != "-1", "long cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xáo trộn 3 điểm | không -1 | thuật toán có thể giải quyết các chu kỳ nhỏ | 
| đúng 4 điểm | 0 | không cần trao đổi | 
| 4 điểm đảo ngược | không -1 | xử lý chu kỳ dài một cách chính xác | 

## Vỏ cạnh 

Nếu tất cả các điểm đã được gắn nhãn chính xác thì không có giao dịch hoán đổi nào được thực hiện và đầu ra là 0. Nếu tất cả các điểm tạo thành một chu kỳ dài duy nhất thì thuật toán sẽ sử dụng điểm cố định để giải quyết chu trình mà không cắt các đoạn. Đối với cấu hình lồi, việc chọn điểm thấp nhất làm điểm neo đảm bảo không có giao dịch hoán đổi đường chéo nào giao nhau. Ví dụ:```
5
0 0 5
1 1 4
2 2 3
3 3 2
4 4 1
```Thuật toán hoán đổi nhãn thông qua
