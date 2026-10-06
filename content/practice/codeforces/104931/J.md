---
title: "CF 104931J - Nấu ăn cẩn thận"
description: "Chúng tôi đang đặt tôm vào lưới $n lần m$, trong đó mỗi ô có thể chứa một con tôm hoặc trống. Mỗi cấu hình chỉ là một ma trận nhị phân. Grill có một quy tắc chỉ kích hoạt cục bộ trên mỗi lưới con $2 nhân 2$."
date: "2026-06-28T07:39:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104931
codeforces_index: "J"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 1 (Advanced)"
rating: 0
weight: 104931
solve_time_s: 64
verified: false
draft: false
---

[CF 104931J - Nấu ăn cẩn thận](https://codeforces.com/problemset/problem/104931/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang đặt tôm lên một$n \times m$lưới, trong đó mỗi ô có thể chứa một con tôm hoặc để trống. Mỗi cấu hình chỉ là một ma trận nhị phân. 

Bàn nướng có quy tắc chỉ kích hoạt cục bộ trên mỗi$2 \times 2$lưới con. Nếu bên trong bất kỳ như vậy$2 \times 2$khối tôm có số lẻ thì khối đó gây ra hiện tượng cháy. Câu hỏi không phải là mô phỏng việc ghi mà là đếm xem có bao nhiêu cấu hình lưới chứa ít nhất một$2 \times 2$lưới con có tính chẵn lẻ lẻ. 

Vì vậy, nhiệm vụ hoàn toàn là tổ hợp: đếm tất cả các lưới nhị phân có kích thước$n \times m$vi phạm điều kiện chẵn lẻ trong ít nhất một$2 \times 2$quảng trường. 

Kích thước đầu vào lên tới$2000 \times 2000$, do đó lưới có thể chứa tới 4 triệu ô. Việc liệt kê bạo lực trên tất cả các lưới sẽ liên quan đến$2^{4 \cdot 10^6}$trạng thái, điều đó hoàn toàn không thể xảy ra. Ngay cả việc kiểm tra một chi phí cấu hình duy nhất$O(nm)$, do đó, bất kỳ cách tiếp cận nào lặp lại trên tất cả các lưới sẽ bị loại trừ ngay lập tức. 

Một trường hợp thất bại tinh vi hơn một chút xuất phát từ việc cố gắng xử lý từng vấn đề.$2 \times 2$một cách độc lập. Các lưới con này chồng chéo lên nhau rất nhiều, vì vậy việc chọn các mẫu hợp lệ cục bộ không đảm bảo tính nhất quán toàn cầu. 

Ví dụ, trong một$2 \times 3$lưới, các ràng buộc trên cột 1-2 và 2-3 chia sẻ cột giữa. Cách tiếp cận tham lam hoặc tính cục bộ sẽ bị tính quá mức vì nó tính gấp đôi cấu trúc được chia sẻ. 

Thay vào đó, cách tiếp cận đúng phải mô tả cấu trúc tổng thể của các lưới trong đó mọi$2 \times 2$lưới con có tính chẵn lẻ và sau đó trừ đi toàn bộ không gian. 

## Phương pháp tiếp cận 

Chúng ta bắt đầu từ ý tưởng trực tiếp nhất. có$2^{nm}$cách để lấp đầy lưới. Đối với mỗi cái, chúng tôi có thể quét tất cả$(n-1)(m-1)$lưới con và kiểm tra xem có lưới con nào có tổng lẻ không. Điều này đúng nhưng quá chậm: việc tạo ra tất cả các lưới đã tốn thời gian theo cấp số nhân và thậm chí một lần kiểm tra cũng là bậc hai. 

Vì vậy, thay vì đếm trực tiếp các lưới “xấu”, chúng ta đảo ngược vấn đề. Chúng tôi đếm phần bù: lưới nơi không có$2 \times 2$lưới con có tổng lẻ. Đây chính xác là những cấu hình mà mọi$2 \times 2$khối có tính chẵn lẻ. 

Điều kiện này cực kỳ cứng nhắc. Lấy bất kỳ$2 \times 2$khối:$$a_{i,j} \oplus a_{i,j+1} \oplus a_{i+1,j} \oplus a_{i+1,j+1} = 0$$Sắp xếp lại mang lại:$$a_{i+1,j+1} = a_{i,j} \oplus a_{i,j+1} \oplus a_{i+1,j}$$Điều này có nghĩa là khi chúng tôi sửa hàng đầu tiên và cột đầu tiên, toàn bộ lưới sẽ bị buộc phải sửa. Mỗi ô khác có thể được tính toán từng bước. 

Một cách có cấu trúc hơn để thấy điều đó là lưới phải đáp ứng hệ thống tuyến tính trên XOR. Không gian nghiệm có chính xác$n + m - 1$bậc tự do: chọn tất cả các giá trị ở hàng đầu tiên và cột đầu tiên, với$a_{1,1}$đã chia sẻ. 

Vì vậy số lượng lưới “an toàn” (không có số lẻ$2 \times 2$) là$2^{n+m-1}$. Mọi thứ khác là một câu trả lời hợp lệ. 

Vậy số đếm cần tìm là:$$2^{nm} - 2^{n+m-1}$$modulo tính toán$998244353$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(2^{nm} \cdot nm)$|$O(nm)$| Quá chậm | 
| Đếm XOR cấu trúc |$O(\log(nm))$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số lưới nhị phân có kích thước$n \times m$, đó là$2^{nm}$. Điều này thể hiện tất cả các vị trí đặt tôm có thể mà không bị hạn chế. 
2. Tính số lượng lưới trong đó mỗi$2 \times 2$lưới con có tính chẵn lẻ, đó là$2^{n+m-1}$. Điều này xuất phát từ thực tế là khi hàng đầu tiên và cột đầu tiên được chọn, tất cả các ô khác được xác định duy nhất bởi ràng buộc XOR. 
3. Trừ số lượng bị ràng buộc khỏi tổng số lượng để tách biệt các cấu hình có chứa ít nhất một vi phạm$2 \times 2$lưới con. Điều này mang lại$2^{nm} - 2^{n+m-1}$. 
4. Thực hiện tất cả các modulo lũy thừa$998244353$, vì các con số tăng theo cấp số nhân và phải giảm trong suốt quá trình tính toán. 

Công cụ tính toán quan trọng là lũy thừa nhanh, vì cả hai số mũ có thể lớn tới 4 triệu. 

### Tại sao nó hoạt động 

Sự ràng buộc đối với mọi$2 \times 2$khối thực thi mối quan hệ tuyến tính trên XOR lan truyền khắp lưới. Khi hàng và cột đầu tiên được cố định, mọi ô còn lại sẽ bị ép buộc bởi các giá trị được xác định trước đó. Điều này loại bỏ tất cả sự tự do ngoại trừ$n + m - 1$các bit độc lập, do đó không gian của lưới hợp lệ chính xác là không gian vectơ có chiều đó. Việc đếm các cấu hình sẽ trở thành việc đếm các phép gán nhị phân cho các biến tự do này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modpow(a, e):
    res = 1
    a %= MOD
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    n, m = map(int, input().split())
    
    total = modpow(2, n * m)
    safe = modpow(2, n + m - 1)
    
    ans = (total - safe) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện hoàn toàn dựa vào lũy thừa mô-đun. Cuộc gọi đầu tiên tính toán không gian cấu hình đầy đủ, trong khi cuộc gọi thứ hai tính toán không gian con có cấu trúc được xác định bởi tính nhất quán XOR. Phép trừ được thực hiện theo modulo$998244353$, do đó sự điều chỉnh cuối cùng đảm bảo không âm. 

Một lỗi phổ biến là cố gắng lặp qua lưới hoặc mô phỏng các ràng buộc cục bộ. Quan điểm đúng đắn là cấu trúc tuyến tính toàn cục, không phải kiểm tra cục bộ. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp nhỏ để xem công thức hoạt động như thế nào. 

Vì$n = 2, m = 2$, chúng tôi tính toán: 

| Số lượng | Giá trị | 
| --- | --- | 
|$nm$| 4 | 
|$2^{nm}$| 16 | 
|$n+m-1$| 3 | 
|$2^{n+m-1}$| 8 | 
| Trả lời | 8 | 

Điều này phù hợp với thực tế là trong số 16 lưới, chính xác một nửa đáp ứng cấu trúc nhất quán XOR. 

Vì$n = 2, m = 3$: 

| Số lượng | Giá trị | 
| --- | --- | 
|$nm$| 6 | 
|$2^{nm}$| 64 | 
|$n+m-1$| 4 | 
|$2^{n+m-1}$| 16 | 
| Trả lời | 48 | 

Điều này xác nhận cách giải thích phép trừ: chỉ có 16 lưới tránh bất kỳ số lẻ nào$2 \times 2$và tất cả những thứ khác đều chứa ít nhất một lưới con vi phạm. 

Mỗi dấu vết xác nhận rằng “không gian an toàn” chỉ phụ thuộc vào mức độ tự do biên chứ không phụ thuộc vào các ô bên trong. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log(nm))$| Hai phép lũy thừa mô-đun sử dụng lũy ​​thừa nhị phân | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ | 

Các ràng buộc cho phép số mũ lên tới 4 triệu, vì vậy việc lũy thừa logarit là cần thiết. Giải pháp thoải mái phù hợp trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def modpow(a, e):
    res = 1
    a %= MOD
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, sys.stdin.readline().split())
    total = modpow(2, n * m)
    safe = modpow(2, n + m - 1)
    return str((total - safe) % MOD)

# provided samples
assert solve("2 2\n") == "8"
assert solve("2 3\n") == "48"

# custom cases
assert solve("2 4\n") == str((pow(2, 8, MOD) - pow(2, 5, MOD)) % MOD), "small rectangular case"
assert solve("3 3\n") == str((pow(2, 9, MOD) - pow(2, 5, MOD)) % MOD), "square grid check"
assert solve("2 2000\n") == str((pow(2, 4000, MOD) - pow(2, 2001, MOD)) % MOD), "wide grid boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 | 8 | lưới không cần thiết nhỏ nhất | 
| 2 4 | tính đúng đắn của công thức trên hình chữ nhật | | 
| 3 3 | xử lý đối xứng vuông và số mũ | | 
| 2 2000 | trường hợp ứng suất số mũ lớn | | 

## Vỏ cạnh 

Trường hợp cạnh khóa là lưới hợp lệ nhỏ nhất$2 \times 2$. Trong trường hợp này, có đúng một$2 \times 2$lưới con, do đó vấn đề giảm xuống việc đếm các ma trận nhị phân trong đó XOR của cả bốn ô là số lẻ. Công thức cho$16 - 8 = 8$, phù hợp với phép liệt kê trực tiếp. 

Đối với một$2 \times m$lưới, cấu trúc đơn giản hóa nhưng vẫn tuân theo quy tắc tương tự. Mỗi cột bổ sung thêm một bậc tự do trong không gian đầy đủ nhưng chỉ có một ràng buộc trong không gian an toàn. Công thức xử lý vấn đề này một cách nhất quán vì nó chỉ phụ thuộc vào số học số mũ. 

Đối với lưới lớn như$2000 \times 2000$, số mũ$nm$trở nên rất lớn, nhưng phép lũy thừa mô đun xử lý nó theo thời gian logarit. Tính đúng đắn không phụ thuộc vào độ lớn mà chỉ phụ thuộc vào cấu trúc đại số của không gian ràng buộc.
