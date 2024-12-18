# Run the full E2E flow by setting the contracts, deploying them, creating a channel, and sending a packet
# Usage: just do-it
do-it:
    echo "Running the full E2E flow..."
    just set-contracts optimism XCounter false && just set-contracts base XCounter false
    just deploy optimism base
    just create-channel
    just send-packet optimism
    echo "You've done it!"
    module.exports = {
    "XCounter": [],
    "XCounterUC": [],
    // Add your contract types here, along with the list of custom constructor arguments
    // DO NOT ADD THE DISPATCHER OR UNIVERSAL CHANNEL HANDLER ADDRESSES HERE!!!
    // These will be added in the deploy script at $ROOT/scripts/deploy.js
};/**
     * @dev Sends a packet with a greeting message over a specified channel, and deposits a fee for relaying the packet
     * @param message The greeting message to be sent.
     * @param channelId The ID of the channel to send the packet to.
     * @param timeoutTimestamp The timestamp at which the packet will expire if not received.
     * @notice If you are relaying your own packets, you should not call this method, and instead call greet.
     * @param gasLimits An array containing two gas limit values:
     *                  - gasLimits[0] for `recvPacket` fees
     *                  - gasLimits[1] for `ackPacket` fees.
     * @param gasPrices An array containing two gas price values:
     *                  - gasPrices[0] for `recvPacket` fees, for the dest chain
     *                  - gasPrices[1] for `ackPacket` fees, for the src chain
     * @notice The total fees sent in the msg.value should be equal to the total gasLimits[0] * gasPrices[0] +
     * @notice Use the Polymer fee estimation api to get the required fees to ensure that enough fees are sent.
     * gasLimits[1] * gasPrices[1]. The transaction will revert if a higher or lower value is sent
     */
    function greetWithFee(
        string calldata message,
        bytes32 channelId,
        uint64 timeoutTimestamp,
        uint256[2] memory gasLimits,
        uint256[2] memory gasPrices
    ) external payable returns (uint64 sequence) {
        sequence = dispatcher.sendPacket(channelId, bytes(message), timeoutTimestamp);
        _depositSendPacketFee(dispatcher, channelId, sequence, gasLimits, gasPrices);
    }
