# smart-contract-cryptorian
import { useState, useEffect } from 'react'
import { Button } from "/components/ui/button"
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "/components/ui/card"
import { Input } from "/components/ui/input"
import { Label } from "/components/ui/label"
import { Avatar, AvatarFallback, AvatarImage } from "/components/ui/avatar"
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "/components/ui/table"
import { Trash, Edit, Plus, Minus, Check, X, ArrowRight } from "lucide-react"

// Mock data
const mockAccountAddress = "0x1234567890abcdef1234567890abcdef12345678"
const mockTokenBalance = 1000
const mockTransactions = [
  { id: 1, to: "0xabcdef1234567890abcdef1234567890abcdef", amount: 100, date: "2023-10-01" },
  { id: 2, to: "0xabcdef1234567890abcdef1234567890abcdef", amount: 200, date: "2023-10-02" },
  { id: 3, to: "0xabcdef1234567890abcdef1234567890abcdef", amount: 50, date: "2023-10-03" },
]

export default function SmartContractDashboard() {
  const [accountAddress, setAccountAddress] = useState(mockAccountAddress)
  const [tokenBalance, setTokenBalance] = useState(mockTokenBalance)
  const [transactions, setTransactions] = useState(mockTransactions)
  const [recipientAddress, setRecipientAddress] = useState("")
  const [transferAmount, setTransferAmount] = useState("")
  const [showSuccessMessage, setShowSuccessMessage] = useState(false)

  useEffect(() => {
    // Simulate fetching account address and token balance
    setAccountAddress(mockAccountAddress)
    setTokenBalance(mockTokenBalance)
    setTransactions(mockTransactions)
  }, [])

  const handleTransfer = () => {
    // Simulate token transfer
    const newTransaction = {
      id: transactions.length + 1,
      to: recipientAddress,
      amount: parseFloat(transferAmount),
      date: new Date().toISOString().split('T')[0],
    }
    setTransactions([...transactions, newTransaction])
    setRecipientAddress("")
    setTransferAmount("")
    setShowSuccessMessage(true)
    setTimeout(() => setShowSuccessMessage(false), 3000)
  }

  return (
    <div className="p-4">
      <Card className="mb-4">
        <CardHeader>
          <CardTitle>User Account</CardTitle>
          <CardDescription>View and manage your account details</CardDescription>
        </CardHeader>
        <CardContent className="flex items-center justify-between">
          <div className="flex items-center space-x-4">
            <Avatar>
              <AvatarImage src="https://github.com/nutlope.png" />
              <AvatarFallback>CN</AvatarFallback>
            </Avatar>
            <div>
              <p className="text-sm font-medium leading-none">Account Address</p>
              <p className="text-sm text-muted-foreground">{accountAddress}</p>
            </div>
          </div>
          <div className="flex items-center space-x-4">
            <p className="text-sm font-medium leading-none">Token Balance</p>
            <p className="text-sm text-muted-foreground">{tokenBalance} Tokens</p>
          </div>
        </CardContent>
      </Card>

      <Card className="mb-4">
        <CardHeader>
          <CardTitle>Transaction History</CardTitle>
          <CardDescription>View your recent transactions</CardDescription>
        </CardHeader>
        <CardContent>
          <Table>
            <TableHeader>
              <TableRow>
                <TableHead>To</TableHead>
                <TableHead>Amount</TableHead>
                <TableHead>Date</TableHead>
              </TableRow>
            </TableHeader>
            <TableBody>
              {transactions.map((transaction) => (
                <TableRow key={transaction.id}>
                  <TableCell>{transaction.to}</TableCell>
                  <TableCell>{transaction.amount} Tokens</TableCell>
                  <TableCell>{transaction.date}</TableCell>
                </TableRow>
              ))}
            </TableBody>
          </Table>
        </CardContent>
      </Card>

      <Card>
        <CardHeader>
          <CardTitle>Transfer Tokens</CardTitle>
          <CardDescription>Send tokens to another address</CardDescription>
        </CardHeader>
        <CardContent>
          <div className="grid w-full items-center gap-4">
            <div className="flex flex-col space-y-1.5">
              <Label htmlFor="recipient-address">Recipient Address</Label>
              <Input
                id="recipient-address"
                value={recipientAddress}
                onChange={(e) => setRecipientAddress(e.target.value)}
                placeholder="0x..."
              />
            </div>
            <div className="flex flex-col space-y-1.5">
              <Label htmlFor="transfer-amount">Amount</Label>
              <Input
                id="transfer-amount"
                value={transferAmount}
                onChange={(e) => setTransferAmount(e.target.value)}
                type="number"
                placeholder="0"
              />
            </div>
            <Button onClick={handleTransfer}>Transfer</Button>
          </div>
        </CardContent>
        {showSuccessMessage && (
          <CardFooter>
            <p className="text-sm text-green-500">Transfer successful!</p>
          </CardFooter>
        )}
      </Card>
    </div>
  )
}
